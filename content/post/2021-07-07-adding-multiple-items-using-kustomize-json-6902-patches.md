---
author: slowe
categories: Tutorial
comments: true
date: 2021-07-07T17:50:00-06:00
tags:
- CAPI
- CLI
- JSON
- Kubernetes
- Kustomize
title: Adding Multiple Items Using Kustomize JSON 6902 Patches
url: /2021/07/07/adding-multiple-items-using-kustomize-json-6902-patches/
---

Recently, I needed to deploy a [Kubernetes][link-1] cluster via [Cluster API (CAPI)][link-2] into a pre-existing AWS VPC. As I outlined in [this post][xref-1] from September 2019, this entails modifying the CAPI manifest to include the VPC ID and any associated subnet IDs, as well as [referencing existing security groups][xref-4] where needed. I knew that I could use [the `kustomize` tool][link-3] to make these changes in a declarative way, as I'd explored [using `kustomize` with Cluster API manifests][xref-2] some time ago. This time, though, I needed to add a list of items, not just modify an existing value. In this post, I'll show you how I used a JSON 6902 patch with `kustomize` to add a list of items to a CAPI manifest.<!--more-->

By the way, if you're not familiar with `kustomize`, you may find [my introduction to `kustomize` post][xref-3] to be helpful. Also, for those readers who are unfamiliar with JSON 6902 patches, [the associated RFC][link-4] is useful, as is [this site][link-5].

In this particular case, the addition of the VPC ID and the subnet IDs were easily handled with a strategic merge patch that referenced the AWSCluster object. More challenging, though, was the reference to the existing security groups that I needed to add (necessary in order to use my existing SSH bastion infrastructure to reach CAPI-instantiated instances). In hindsight, I suppose I _could_ have written a single strategic merge patch to do the same thing, but I went down the JSON 6902 patch route (and learned something along the way).

Here's a snippet of what the final (rendered) YAML needed to look like (taken from an AWSMachineTemplate object in the CAPI manifest, the security group IDs are obviously fake):

```yaml
spec:
  template:
    spec:
      additionalSecurityGroups:
        - id: sg-01234567890123456
        - id: sg-abcdef0123456789a
```

The knowledge I was missing to make this work was how to correctly format the JSON 6902 patch such that it would add a list of items when rendered using `kustomize`. The [jsonpatch.com][link-5] site gave me the hint I needed---it just needed to be formatted like a JSON list. (It's so obvious to me now, but I guess lots of things seem obvious after we learn them, right?)

Here's a JSON 6902 patch that would create the YAML I was seeking:

```json
[
  { "op": "add",
    "path": "/spec/template/spec/additionalSecurityGroups",
    "value": [
        { "id": "sg-01234567890123456" },
        { "id": "sg-abcdef0123456789a" }
    ]
  }
]
```

The second useful piece to glean from this is something that I also mentioned in [the post on using `kustomize` with CAPI][xref-2], and that's the use of a regular expression (regex) in the `kustomization.yaml` file to control where this JSON 6902 patch is applied. As outlined in [the previous post on using existing AWS security groups with CAPI][xref-4], the `additionalSecurityGroups` field can be added to an AWSMachineTemplate---this is exactly what I wanted so that both my MachineDeployments _and_ my KubeadmControlPlane would pick up the change. So how do I get one patch to apply to multiple objects? With a regex, of course!

```yaml
patchesJson6902:
  - path: securitygroups.json
    target:
      group: infrastructure.cluster.x-k8s.io
      kind: AWSMachineTemplate
      name: ".*"
      version: v1alpha3
```

The use of the `.*` regex for the `name` field means that `kustomize` will apply the referenced JSON 6902 patch to all objects that match the specified API group, kind, and API version. This is an extremely useful way to apply patches, and it's not limited to JSON 6902 patches (which is why I said earlier I could have written a strategic merge patch instead).

Once I had the JSON 6902 patch file in place and the necessary changes to the `kustomization.yaml` file made, running `kustomize build .` created _exactly_ the YAML I needed. Once I'd verified the correctness of the `kustomize build` ouptut, a quick `kustomize build . | kubectl apply -f -` set the creation of my workload cluster in action.

I hope this information proves useful to someone. If you have questions, or if you spot a mistake I've made in this post, feel free to reach out. I'm always open to constructive feedback. You can contact [me on Twitter][link-6] (DMs are open) or hit me up on [the Kubernetes Slack][link-7]. I'd love to hear from you.

[link-1]: https://kubernetes.io
[link-2]: https://cluster-api.sigs.k8s.io
[link-3]: https://kustomize.sigs.k8s.io/
[link-4]: https://datatracker.ietf.org/doc/html/rfc6902/
[link-5]: http://jsonpatch.com/
[link-6]: https://twitter.com/scott_lowe
[link-7]: https://kubernetes.slack.com
[xref-1]: {{< relref "2019-09-09-consuming-preexisting-aws-infrastructure-with-cluster-api.md" >}}
[xref-2]: {{< relref "2019-11-12-using-kustomize-with-cluster-api.md" >}}
[xref-3]: {{< relref "2019-09-13-an-introduction-to-kustomize.md" >}}
[xref-4]: {{< relref "2020-04-22-using-existing-aws-security-groups-with-cluster-api.md" >}}
