---
author: slowe
categories: Tutorial
comments: true
date: 2021-03-19T08:45:00-06:00
tags:
- CAPI
- CLI
- Kubernetes
- Kustomize
title: Adding a MachineHealthCheck using Kustomize
url: /2021/03/19/adding-a-machinehealthcheck-with-kustomize/
---

MachineHealthChecks are a powerful feature in the Kubernetes [Cluster API][link-1] (CAPI), and something I played around with not too long ago on [TGIK 143][link-2]. Recently, I was helping to document the use of [`kustomize`][link-3] with Cluster API for inclusion in the upstream CAPI documentation, and I learned a simple trick with `kustomize` that I'd apparently overlooked in the past. If you've used `kustomize` for any great length of time you probably already know and have used the functionality I'll describe in this post, but if you're new to `kustomize` or, like me, a user of `kustomize` that hasn't had time to dig into all of its functionality, then read on and see how you can use `kustomize` to add a MachineHealthCheck to a CAPI workload cluster.<!--more-->

If you're not familiar with `kustomize`, then reading [my introduction to `kustomize`][xref-1] may be useful before continuing on with the rest of this article.

In this use case---adding a MachineHealthCheck to an workload cluster in CAPI---I'll work from the assumption that you have a "base" CAPI workload cluster definition (perhaps one you've generated using `clusterctl config cluster`). In the directory where this workload cluster manifest exists, you'd need to add a `kustomization.yaml` to specify resources. It would look something like this:

```yaml
---
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - base.yaml
```

Now, let's say you want to add a MachineHealthCheck for this workload cluster. You'd create a `kustomize` overlay directory, and in that overlay directory you'd place (at least) two files:

1. Another `kustomization.yaml` file (more on that in a moment)
2. A YAML manifest for a MachineHealthCheck

(I say "at least" two files because you could also place other patches or other resources in the directory as well.)

The YAML manifest for the MachineHealthCheck is straightforward; I'll only point out to make sure to specify the correct cluster name and deployment name, taking into account any "namePrefix" or "nameSuffix" directives you may be using.

The `kustomization.yaml` would look something like this:

```yaml
---
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - ../../base
  - workload-mhc.yaml
```

Now, you may also include various other directives, but the key here is in the "resources" section. It does, of course, specify the base configuration, but it also lists the MachineHealthCheck manifest that resides in this overlay directory. When you run `kustomize build .`, `kustomize` will _combine_ the specified resources together. In this case, that means it will combine the base workload cluster manifest and the MachineHealthCheck manifest, and the end result---when you feed this to `kubectl apply`---will be a new workload cluster _and_ a MachineHealthCheck to go along with it.

The functionality of combining resources in an overlay is a core part of the functionality of `kustomize`, but for some reason I hadn't leveraged it yet. Kudos to the Cluster API Provider for Azure (CAPZ) team for illustrating this use case in the creation of [workload cluster template "flavors."][link-4] Now that I know it's there, I can begin to see other potential use cases, such as adding extra MachineDeployments to a base workload cluster configuration.

I hope this information is useful. As I said, if you're a long-time `kustomize` user, this is probably not news to you, but for others who are still exploring all the various pieces of functionality that `kustomize` offers I hope this opens up some new possibilities for you. I welcome all constructive feedback; feel free to reach out to me on [the Kubernetes Slack instance][link-5] or contact [me on Twitter][link-6].

[link-1]: https://cluster-api.sigs.k8s.io
[link-2]: https://tgik.io/143
[link-3]: https://github.com/kubernetes-sigs/kustomize/
[link-4]: https://github.com/kubernetes-sigs/cluster-api-provider-azure/tree/master/templates/flavors
[link-5]: https://kubernetes.slack.com
[link-6]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-09-13-an-introduction-to-kustomize.md" >}}
