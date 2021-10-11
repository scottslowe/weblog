---
author: slowe
categories: Explanation
comments: true
date: 2020-03-13T09:55:00-07:00
tags:
- CAPI
- CLI
- Kubernetes
- Kustomize
- YAML
title: Configuring Kustomize Transformers for Cluster API
url: /2020/03/13/configuring-kustomize-transformers-for-cluster-api/
---

In November 2019 I wrote an article on [using `kustomize` with Cluster API (CAPI) manifests][xref-1]. The idea was to use `kustomize` to simplify the management of CAPI manifests for clusters that are generally similar but have minor differences (like the AWS region in which they are running, or the number of Machines in a MachineDeployment). In this post, I'd like to show a slightly different way of using `kustomize` with Cluster API that involves configuring the `kustomize` transformers.<!--more-->

If you aren't familiar with `kustomize`, I'd recommend having a look at [the `kustomize` web site][link-4] and/or reading [my introductory post][xref-2]. A _transformer_ in `kustomize` is the part that is responsible for modifying a resource, or gathering information about a resource over the course of a `kustomize build` process. This page has some useful [terminology definitions][link-3].

Looking back at [the earlier article][xref-1] on using `kustomize` with CAPI, you can see that---due to the links/references between objects---modifying the name of the AWSCluster object also means modifying the reference to the AWSCluster object from the Cluster object. The same goes for the KubeadmConfigTemplate and AWSMachineTemplate objects referenced from a MachineDeployment. Out of the box, the `namePrefix` transformer _will_ change the names of these objects (because they all have a `metadata.name` field), but it _will not_ change the references to these objects. Thus, modifying the names of all the objects becomes an exercise in writing multiple patch files (somewhere around 15-20 patch files).

As it turns out, though, there's a way to fix this by configuring the transformers within `kustomize` (more information [here][link-2]).

The `nameReference` transformer within `kustomize` is used to track references between objects like this, so that when the name of an object is changed---say, using the `namePrefix` transformer---then `kustomize` knows all the other places it needs to change as well. By default, this transformer isn't aware of CAPI objects, and thus doesn't know how (or that it should) update references to other objects. By configuring this transformer, users can change that behavior.

Here's how you can give the `nameReference` transformer a bit more awareness of CAPI objects so it does know how to update references to objects.

First, get the default transformer configurations by saving them with the `kustomize config save -d <directory>` command. The specific file you need is the `namereference.yaml` file created by that command. You can discard the others, unless you plan to make modifications to other transformers. There is one other configuration change I'll recommend, so don't ditch all the files just yet.

Second, edit the `namereference.yaml` file to include this:

```yaml
- kind: AWSCluster
  group: infrastructure.cluster.x-k8s.io
  version: v1alpha2
  fieldSpecs:
  - path: spec/infrastructureRef/name
    kind: Cluster

- kind: AWSMachine
  group: infrastructure.cluster.x-k8s.io
  version: v1alpha2
  fieldSpecs:
  - path: spec/infrastructureRef/name
    kind: Machine

- kind: KubeadmConfig
  group: bootstrap.cluster.x-k8s.io
  version: v1alpha2
  fieldSpecs:
  - path: spec/bootstrap/configRef/name
    kind: Machine

- kind: AWSMachineTemplate
  group: infrastructure.cluster.x-k8s.io
  version: v1alpha2
  fieldSpecs:
  - path: spec/template/spec/infrastructureRef/name
    kind: MachineDeployment

- kind: KubeadmConfigTemplate
  group: bootstrap.cluster.x-k8s.io
  version: v1alpha2
  fieldSpecs:
  - path: spec/template/spec/bootstrap/configRef/name
    kind: MachineDeployment
```

Third, add this line to your `kustomization.yaml` file (see [here][link-1] for more information on using configurations):

```yaml
configurations:
- /path/to/customized/namereference.yaml
```

Now, when you use something like `namePrefix` in your `kustomization.yaml` file for an overlay, not only will the `metadata.name` fields of all objects get transformed, but the _references_ to those objects will also be updated. So, the reference from the Cluster object to the AWSCluster object will be updated. The reference from the MachineDeployment object to the KubeadmConfigTemplate and AWSMachineTemplate objects will be updated. The reference from a Machine object to the KubeadmConfig object will be updated.

With this one change to the `nameReference` transformer, you can go from individually patching objects to using a single `namePrefix` or `nameSuffix` transformer and having all objects appropriately updated. This greatly simplifies the use of `kustomize` with CAPI.

There's a second change I'd also recommend you consider. The `commonLabels` transformer is used to add labels to objects. By default, this transformer doesn't know how to add labels to a MachineDeployment, and this means that you can't use the `commonLabels` transformer to modify the labels used by a MachineDeployment in your CAPI manifest. Instead, you'd have to manually patch these objects. (This is necessary because CAPI uses labels to provide cluster membership information. Change the name of the cluster, and you have to change the labels as well.)

To fix this, edit the `commonlabels.yaml` file generated by `kustomize config save` to add this content:

```yaml
- path: spec/selector/matchLabels
  create: true
  group: cluster.x-k8s.io
  kind: MachineDeployment

- path: spec/template/metadata/labels
  create: true
  group: cluster.x-k8s.io
  kind: MachineDeployment
```

Now the `commonLabels` transformer knows that labels have to be added to the `spec.selector.matchLabels` and `spec.template.metadata.labels` fields for MachineDeployment objects. This keeps you from having to patch individual objects; instead, you can use a single `commonLabels` entry in your `kustomization.yaml` file (you also have to reference the customized configuration using a `configurations` line as shown above). Sweet!

I'm still exploring what other changes may be beneficial, but these are the two I've found (so far) that have had the most impact on the usability of `kustomize` with CAPI. If I find more, I'll either update this post or publish a follow-up post.

The information shared above is for Cluster API v1alpha2; for information on configuring `kustomize` transfomers for use with Cluster API v1alpha3, see [this follow-up post][xref-3]. For information on configuring `kustomize` transformers for use with Cluster API v1beta1, see [this post][xref-4].

If you have questions, comments, suggestions for improvement, or corrections, please let me know. Feel free to [contact me on Twitter][link-5], reach out to me on [the Kubernetes Slack instance][link-6], or hit me up via e-mail (my address isn't too hard to find/figure out).

[link-1]: https://kubectl.docs.kubernetes.io/pages/reference/kustomize.html#configurations
[link-2]: https://github.com/kubernetes-sigs/kustomize/tree/master/examples/transformerconfigs
[link-3]: https://github.com/kubernetes-sigs/kustomize/blob/master/docs/glossary.md
[link-4]: https://kustomize.io/
[link-5]: https://twitter.com/scott_lowe
[link-6]: https://kubernetes.slack.com
[xref-1]: {{< relref "2019-11-12-using-kustomize-with-cluster-api.md" >}}
[xref-2]: {{< relref "2019-09-13-an-introduction-to-kustomize.md" >}}
[xref-3]: {{< relref "2020-03-17-kustomize-transformer-configuration-cluster-api-v1alpha3.md" >}}
[xref-4]: {{< relref "2021-10-11-kustomize-transformer-configurations-for-cluster-api-v1beta1.md" >}}
