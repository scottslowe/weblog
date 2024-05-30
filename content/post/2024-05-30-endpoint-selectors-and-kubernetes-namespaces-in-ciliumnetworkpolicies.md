---
author: slowe
categories: Explanation
comments: true
date: 2024-05-30T16:30:00-06:00
tags:
- Cilium
- Kubernetes
- Networking
- Security
title: Endpoint Selectors and Kubernetes Namespaces in CiliumNetworkPolicies
url: /2024/05/30/endpoint-selectors-and-kubernetes-namespaces-in-ciliumnetworkpolicies/
---

While performing some testing with CiliumNetworkPolicies, I came across a behavior that was unintuitive and unexpected to me. The behavior centers around how an endpoint selector behaves in a CiliumNetworkPolicies when Kubernetes namespaces are involved. (If you didn't understand a bit of what I just said,  I'll provide some additional explanation shortly---stay with me!) After chatting through the behavior with a few folks, I realized the behavior is essentially "correct" and expected. However, if I was confused by the behavior then there's a good chance others might be confused by the behavior as well, so I thought a quick blog post might be a good idea. Keep reading to get more details on the interaction between endpoint selectors and Kubernetes namespaces in CiliumNetworkPolicies.<!--more-->

Before digging into the behavior, let me first provide some definitions or explanations of the various things involved here:

* Kubernetes _namespaces_ are a way to logically isolate groups of resources in a cluster. For example, you might install the software that drives your point-of-sale (PoS) devices in the "retail-pos" namespace while the application that handles inventory is in the "inventory" namespace. You can read more [about namespaces in the Kubernetes documentation][link-1].
* _CiliumNetworkPolicies_ are Cilium-specific network policies that offer additional functionality above and beyond the standard Kubernetes network policies. The Cilium documentation has [more information on CiliumNetworkPolicies][link-3], and read [about standard network policies in the Kubernetes documentation][link-2].
* _Endpoint selectors_ are part of CiliumNetworkPolicies; they are similar to Kubernetes LabelSelectors, but operate on Endpoints (another Cilium-specific object). See [the Cilium docs on Rule Basics][link-4] for more details.

With these high-level definitions in mind, let's dig into the behavior. I'll start with this short CiliumNetworkPolicy (taken directly [from the Cilium docs][link-5]):

```yaml
apiVersion: "cilium.io/v2"
kind: CiliumNetworkPolicy
metadata:
  name: "allow-all-to-victim"
spec:
  endpointSelector:
    matchLabels:
      role: victim
  ingress:
  - fromEndpoints:
    - {}
```

This policy is described as a rule that will allow all inbound traffic to the endpoints that match the `endpointSelector`. Indeed, the rule has an empty `fromEndpoints` rule (the `- {}` under `fromEndpoints` in the example above), which does indeed select all endpoints.

Now look at this policy (also taken [directly from the Cilium documentation][link-7]):

```yaml
apiVersion: "cilium.io/v2"
kind: CiliumNetworkPolicy
metadata:
  name: "isolate-ns1"
  namespace: ns1
spec:
  endpointSelector:
    matchLabels:
      {}
  ingress:
  - fromEndpoints:
    - matchLabels:
        {}
```

Note the similarity in the `ingress` section of this example policy versus the first one---this policy has an empty `matchLabels` set on the `fromEndpoints`, meaning it will match any set of labels on an endpoint (essentially, matching all endpoints). Because the `fromEndpoints` matches all endpoints, like the first policy, you might look at this policy and think it is also intended to allow all inbound traffic.

However, this policy is described as a policy to "lock down ingress of the pods" in the specified namespace. Huh? How can two policies, both of which have empty `fromEndpoint` rules, both allow all inbound traffic and also lock down inbound traffic?

The key here is understanding the interaction between CiliumNetworkPolicies and Kubernetes namespaces. The Cilium docs have a page [discussing the use of Kubernetes constructs in policies][link-6], and on that page it reminds you that CiliumNetworkPolicies are namespaced-scoped (limited or constrained to a single namespace). What this means is that the empty `fromEndpoints` rule in the policy example above **does** select all endpoints---but it's only _all endpoints in the namespace in which the policy is applied._

When you add that missing piece---that an empty endpoint selector only select endpoints in the same/specified namespace---then understanding these two example policies makes more sense. Both policies accomplish the same thing: they both _allow all traffic_ from the current namespace while _denying traffic_ from other namespaces. (The first policy is, in my opinion, a bit inaccurately described. It does allow all inbound traffic, but only from the current namespace.)

So what's the key takeaway here? If you need to write a CiliumNetworkPolicy that targets endpoints in another namespace, you can't use an empty `fromEndpoints` rule---instead, you need to explicitly call out the namespace of the source endpoint, like this:

```yaml
apiVersion: "cilium.io/v2"
kind: CiliumNetworkPolicy
metadata:
  name: "k8s-expose-across-namespace"
  namespace: ns1
spec:
  endpointSelector:
    matchLabels:
      {}
  ingress:
  - fromEndpoints:
    - matchLabels:
        k8s:io.kubernetes.pod.namespace: ns2
```

I hope this explanation is helpful. As I mentioned at the start of the post, if I was confused by this then there's a reasonable chance that others are (or will be) confused by it as well. I do intend to submit one or more PRs to the Cilium documentation to see if I can help clarify this in some way. In the meantime, if you have any questions---or any feedback for me---feel free to reach out. You can find me [on Twitter][link-8], [in the Fediverse][link-9], or in a number of different Slack communities. Thanks for reading!

[link-1]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[link-2]: https://kubernetes.io/docs/concepts/services-networking/network-policies/
[link-3]: https://docs.cilium.io/en/v1.15/network/kubernetes/policy/#ciliumnetworkpolicy
[link-4]: https://docs.cilium.io/en/v1.15/security/policy/intro/#rule-basics
[link-5]: https://docs.cilium.io/en/v1.15/security/policy/language/#ingress-allow-all-endpoints
[link-6]: https://docs.cilium.io/en/stable/security/policy/kubernetes/
[link-7]: https://docs.cilium.io/en/stable/security/policy/kubernetes/#example-enforce-namespace-boundaries
[link-8]: https://twitter.com/scott_lowe
[link-9]: https://fosstodon.org/@scottslowe
