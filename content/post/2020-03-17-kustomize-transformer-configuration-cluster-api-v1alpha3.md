---
author: slowe
categories: Information
comments: true
date: 2020-03-17T15:42:00-07:00
tags:
- CAPI
- CLI
- Kubernetes
- Kustomize
- YAML
title: Kustomize Transformer Configurations for Cluster API v1alpha3
url: /2020/03/17/kustomize-transformer-configuration-cluster-api-v1alpha3/
---

A few days ago I wrote an article on [configuring `kustomize` transformers][xref-1] for use with [Cluster API (CAPI)][link-1], in which I explored how users could configure the `kustomize` transformers---the parts of `kustomize` that actually modify objects---to be a bit more CAPI-aware. By doing so, using `kustomize` with CAPI manifests becomes much easier. Since that post, the CAPI team released v1alpha3. In working with v1alpha3, I realized my `kustomize` transformer configurations were incorrect. In this post, I will share CAPI v1alpha3 configurations for `kustomize` transformers.<!--more-->

In [the previous post][xref-1], I referenced changes to both `namereference.yaml` (to configure the nameReference transformer) and `commonlabels.yaml` (to configure the commonLabels transformer). CAPI v1alpha3 has changed the default way labels are used with MachineDeployments, so for v1alpha3 you _may_ be able to get away with only changes to `namereference.yaml`. (If you know you are going to want/need additional labels on your MachineDeployment, then plan on changes to `commonlabels.yaml` as well.)

Here are the CAPI v1alpha3 changes needed to `namereference.yaml`:

```yaml
- kind: Cluster
  group: cluster.x-k8s.io
  version: v1alpha3
  fieldSpecs:
  - path: spec/clusterName
    kind: MachineDeployment
  - path: spec/template/spec/clusterName
    kind: MachineDeployment

- kind: AWSCluster
  group: infrastructure.cluster.x-k8s.io
  version: v1alpha3
  fieldSpecs:
  - path: spec/infrastructureRef/name
    kind: Cluster

- kind: KubeadmControlPlane
  group: controlplane.cluster.x-k8s.io
  version: v1alpha3
  fieldSpecs:
  - path: spec/controlPlaneRef/name
    kind: Cluster

- kind: AWSMachine
  group: infrastructure.cluster.x-k8s.io
  version: v1alpha3
  fieldSpecs:
  - path: spec/infrastructureRef/name
    kind: Machine

- kind: KubeadmConfig
  group: bootstrap.cluster.x-k8s.io
  version: v1alpha3
  fieldSpecs:
  - path: spec/bootstrap/configRef/name
    kind: Machine

- kind: AWSMachineTemplate
  group: infrastructure.cluster.x-k8s.io
  version: v1alpha3
  fieldSpecs:
  - path: spec/template/spec/infrastructureRef/name
    kind: MachineDeployment
  - path: spec/infrastructureTemplate/name
    kind: KubeadmControlPlane

- kind: KubeadmConfigTemplate
  group: bootstrap.cluster.x-k8s.io
  version: v1alpha3
  fieldSpecs:
  - path: spec/template/spec/bootstrap/configRef/name
    kind: MachineDeployment
```

You also need to include this line in your `kustomization.yaml` (to bring in the transformer configuration):

```yaml
configurations:
- /path/to/customized/namereference.yaml
```

If you do need to edit `commonlabels.yaml`, the changes for CAPI v1alpha3 are the same as described in [the previous article][xref-1]. You'll then need to add an entry to your `configurations:` line, as noted above.

For information on configuring `kustomize` transformers to support the v1beta1 version of Cluster API, please see [this follow-up post][xref-2].

I hope this helps! Find [me on Twitter][link-2] or on [the Kubernetes Slack instance][link-3] if you have any questions, comments, or corrections.

[link-1]: https://cluster-api.sigs.k8s.io/introduction.html
[link-2]: https://twitter.com/scott_lowe
[link-3]: https://kubernetes.slack.com
[xref-1]: {{< relref "2020-03-13-configuring-kustomize-transformers-for-cluster-api.md" >}}
[xref-2]: {{< relref "2021-10-11-kustomize-transformer-configurations-for-cluster-api-v1beta1.md" >}}
