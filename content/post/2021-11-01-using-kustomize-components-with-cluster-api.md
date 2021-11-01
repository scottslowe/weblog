---
author: slowe
categories: Tutorial
comments: true
date: 2021-11-01T10:00:00-06:00
tags:
- CAPI
- CLI
- Kubernetes
- Kustomize
title: Using Kustomize Components with Cluster API
url: /2021/11/01/using-kustomize-components-with-cluster-api/
---

I've been using [Kustomize][link-1] with [Cluster API][link-2] (CAPI) to manage my AWS-based [Kubernetes][link-3] clusters for quite a while (along with [Pulumi][link-4] for managing the underlying AWS infrastructure). For all the time I've been using this approach, I've also been unhappy with the overlay-based approach that had evolved as a way of managing multiple workload clusters. With the recent release of CAPI 1.0 and the v1beta1 API, I took this opportunity to see if there was a better way. I found a different way---time will tell if it is a better way. In this post, I'll share how I'm using Kustomize components to help streamline managing multiple CAPI workload clusters.<!--more-->

Before continuing, I feel it's important to point out that while the bulk of the Kustomize API is reasonably stable at v1beta1, the components portion of the API is still in early days (v1alpha1). So, if you adopt this functionality, be aware that it may change (or even get dropped).

More information on Kustomize components can be found in [the Kustomize components KEP][link-5] or in [this demo document][link-6]. The [documentation on Kustomize components][link-7] is somewhat helpful as well. I won't try to rehash information found in those sources here, but instead build on that information with a CAPI-focused use case. Finally, if you're unfamiliar with using Kustomize with CAPI, start with [this introduction][xref-1] and then read the post on [transformer configurations for v1beta1][xref-2].

So, what are Kustomize components? In the context of CAPI, let's say that you have a discrete change or configuration you'd like to make to a base CAPI manifest. Perhaps it's changing the `imageLookupBaseOS` setting, as described in [this post][xref-3], to influence AMI selection. You could use a JSON 6902 patch, similar to this, to make that change:

```json
[
  { "op": "add",
    "path": "/spec/template/spec/imageLookupBaseOS",
    "value": "ubuntu-20.04"
  }
]
```

You could turn this into a Kustomize component using the following `kustomization.yaml`:

```yaml
---
apiVersion: kustomize.config.k8s.io/v1alpha1
kind: Component
patchesJson6902:
  - path: spec-template-spec-base-os.json
    target:
      group: infrastructure.cluster.x-k8s.io
      kind: AWSMachineTemplate
      name: ".*"
      version: v1beta1
  - path: spec-template-spec-base-os.json
    target:
      group: infrastructure.cluster.x-k8s.io
      kind: AWSMachine
      name: ".*"
      version: v1beta1
```

Note the `kind: Component` and the v1alpha1 API version, which distinguish this file from a typical `kustomization.yaml`. This file references the JSON 6902 patch shared earlier and applies that patch to all AWSMachine and AWSMachineTemplate objects.

Congratulations, you've defined your first component! Now, what do you do with it?

To use a component you've defined, simply include it in a `kustomization.yaml` like this:

```yaml
---
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - ../base
components:
  - ../components/ubuntu-20.04
```

This is a more "traditional" `kustomization.yaml` that specifies a base resource, but then instead of referencing a list of transformers or generators or patches, it references the component you defined earlier. You are, of course, not limited to using just one component. Here's an example from my own lab configuration files:

```yaml
---
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - ../capi-base
components:
  - ../components/dev
  - ../components/us-west-2
  - ../components/calico-cni
  - ../components/aws-ccm
  - ../components/aws-ebs-csi
  - ../components/ubuntu-20.04
```

In this configuration, I'm referencing a number of different components:

* The "dev" component controls the number of MachineDeployments and how many replicas each MachineDeployment manages. If I need more MachineDeployments with more replicas, I can simply swap out this component for the "prod" component.
* The "calico-cni" component adds labels that cause the workload cluster to match against a ClusterResourceSet, thereby automatically installing the Calico CNI plugin. (Curious about ClusterResourceSets? Read [this][xref-4] or [this][xref-5].)
* Similarly, the "aws-ccm" and "aws-ebs-csi" components add labels for ClusterResourceSets that install the external AWS cloud provider and EBS CSI driver, respectively. (See [this post][xref-6] for more details on using the external AWS cloud provider.)
* The "ubuntu-20.04" component sets the AMIs to Ubuntu 20.04.

The awesome thing about using components is that I can define the change I want just once---as a component---and then reference it from multiple overlays. Each overlay becomes a list of references to components, instead of slightly-different copies of other overlays. This is a _huge_ improvement over duplicating patches for multiple workload clusters.

Overall, I'm far more pleased with using components to describe Kustomize overlays for CAPI workload clusters than previous approaches.

I hope this is helpful to others. If you have any questions, want to talk about it in more detail, or have suggestions for how I can improve, please don't hesitate to contact me. Reach out to [me on Twitter][link-8], or find me on [the Kubernetes Slack][link-9] community.

[link-1]: https://kustomize.io
[link-2]: https://cluster-api.sigs.k8s.io
[link-3]: https://kubernetes.io
[link-4]: https://www.pulumi.com
[link-5]: https://github.com/kubernetes/enhancements/blob/master/keps/sig-cli/1802-kustomize-components/README.md
[link-6]: https://github.com/kubernetes-sigs/kustomize/blob/master/examples/components.md
[link-7]: https://kubectl.docs.kubernetes.io/guides/config_management/components/
[link-8]: https://twitter.com/scott_lowe
[link-9]: https://kubernetes.slack.com
[xref-1]: {{< relref "2019-11-12-using-kustomize-with-cluster-api.md" >}}
[xref-2]: {{< relref "2021-10-11-kustomize-transformer-configurations-for-cluster-api-v1beta1.md" >}}
[xref-3]: {{< relref "2021-10-25-influencing-cluster-api-ami-selection.md" >}}
[xref-4]: {{< relref "2021-03-02-deploying-a-cni-automatically-with-a-clusterresourceset.md" >}}
[xref-5]: {{< relref "2021-10-07-installing-cilium-via-clusterresourceset.md" >}}
[xref-6]: {{< relref "2021-10-12-using-the-external-aws-cloud-provider-for-kubernetes.md" >}}
