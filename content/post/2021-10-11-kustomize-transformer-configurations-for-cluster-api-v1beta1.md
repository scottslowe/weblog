---
author: slowe
categories: Information
comments: true
date: 2021-10-11T16:00:00-06:00
tags:
- CAPI
- CLI
- Kubernetes
- Kustomize
- YAML
title: Kustomize Transformer Configurations for Cluster API v1beta1
url: /2021/10/11/kustomize-transformer-configurations-for-cluster-api-v1beta1/
---

The topic of combining [`kustomize`][link-1] with [Cluster API][link-2] (CAPI) is a topic I've touched on several times over the last 18-24 months. I first touched on this topic in November 2019 with a post on [using `kustomize` with CAPI manifests][xref-1]. A short while later, I discovered a way to change the configurations for the `kustomize` transformers to make it easier to use it with CAPI. That resulted in two posts on changing the `kustomize` transformers: [one for v1alpha2][xref-2] and [one for v1alpha3][xref-3] (since there were changes to the API between versions). In this post, I'll revisit `kustomize` transformer configurations again, this time for CAPI v1beta1 (the API version corresponding to the CAPI 1.0 release).<!--more-->

In [the v1alpha2 post][xref-2] (the first post on modifying `kustomize` transformer configurations), I mentioned that changes were needed to the NameReference and CommonLabel transformers. In [the v1alpha3 post][xref-3], I mentioned that the changes to the CommonLabel transformer became largely optional; if you are planning on adding additional labels to MachineDeployments, then the change to the CommonLabels transformer is required, but otherwise you could probably get by without it.

For v1beta1, the necessary changes are _very_ similar to v1alpha3, and (for the most part) are focused on the NameReference transformer. The NameReference transformer tracks references between objects, so that if the name of an object changes---perhaps due to use of the `namePrefix` or `nameSuffix` directives in the `kustomization.yaml` file---references to that object are also appropriately renamed.

Here are the CAPI-related changes needed for the NameReference transformer:

```yaml
- kind: Cluster
  group: cluster.x-k8s.io
  version: v1beta1
  fieldSpecs:
  - path: spec/clusterName
    kind: MachineDeployment
  - path: spec/template/spec/clusterName
    kind: MachineDeployment

- kind: AWSCluster
  group: infrastructure.cluster.x-k8s.io
  version: v1beta1
  fieldSpecs:
  - path: spec/infrastructureRef/name
    kind: Cluster

- kind: KubeadmControlPlane
  group: controlplane.cluster.x-k8s.io
  version: v1beta1
  fieldSpecs:
  - path: spec/controlPlaneRef/name
    kind: Cluster

- kind: AWSMachine
  group: infrastructure.cluster.x-k8s.io
  version: v1beta1
  fieldSpecs:
  - path: spec/infrastructureRef/name
    kind: Machine

- kind: KubeadmConfig
  group: bootstrap.cluster.x-k8s.io
  version: v1beta1
  fieldSpecs:
  - path: spec/bootstrap/configRef/name
    kind: Machine

- kind: AWSMachineTemplate
  group: infrastructure.cluster.x-k8s.io
  version: v1beta1
  fieldSpecs:
  - path: spec/template/spec/infrastructureRef/name
    kind: MachineDeployment
  - path: spec/machineTemplate/infrastructureRef/name
    kind: KubeadmControlPlane

- kind: KubeadmConfigTemplate
  group: bootstrap.cluster.x-k8s.io
  version: v1beta1
  fieldSpecs:
  - path: spec/template/spec/bootstrap/configRef/name
    kind: MachineDeployment
```

Generally, you'd append this content to the default NameReference transformer configuration, which you'd obtain using `kustomize config save`. However, somewhere in the Kustomize 3.8.4 release timeframe, the `kustomize config save` command for extracting the default transformer configurations was removed, and I have yet to figure out another way of getting this information. In theory, when using `kustomize` with CAPI manifests, you wouldn't need any of the default NameReference transformer configurations, but I haven't conducted any thorough testing of that theory (yet).

Aside from replacing all instances of `v1alpha3` with `v1beta1`, the only other difference in the YAML shown above compared to YAML in the [the v1alpha3 post][xref-3] is a change to the `fieldSpecs` list for AWSMachineTemplate. Previously, the KubeadmControlPlane referenced an underlying AWSMachineTemplate at the path `spec/infrastructureTemplate/name`. In v1beta1, the KubeadmControlPlane object now references an AWSMachineTemplate at the path `spec/machineTemplate/infrastructureRef/name`.

As mentioned in both of the previous posts, you'll need to put this content in a file (I use `namereference.yaml`) and then specify the path to this configuration in `kustomization.yaml`, like this:

```yaml
configurations:
  - /path/to/customized/namereference.yaml
```

I hope this information is useful to readers. Feel free to find me on [the Kubernetes Slack instance][link-3] if you have questions, and I'll do my best to help answer them. You're also welcome to contact [me on Twitter][link-4] (DMs are open). Thanks!

[link-1]: https://kustomize.io/
[link-2]: https://cluster-api.sigs.k8s.io/
[link-3]: https://kubernetes.slack.com
[link-4]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-11-12-using-kustomize-with-cluster-api.md" >}}
[xref-2]: {{< relref "2020-03-13-configuring-kustomize-transformers-for-cluster-api.md" >}}
[xref-3]: {{< relref "2020-03-17-kustomize-transformer-configuration-cluster-api-v1alpha3.md" >}}
