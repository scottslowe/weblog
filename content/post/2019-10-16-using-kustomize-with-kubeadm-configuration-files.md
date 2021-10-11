---
aliases: /2019/10/16/using-kustomize-with-cluster-api/
author: slowe
categories: Explanation
comments: true
date: 2019-10-16T10:00:00-07:00
tags:
- CLI
- Kubeadm
- Kubernetes
- Kustomize
- YAML
title: Using Kustomize with Kubeadm Configuration Files
url: /2019/10/16/using-kustomize-with-kubeadm-configuration-files/
---

Last week I had a crazy idea: if [`kustomize`][link-1] can be used to modify YAML files like [Kubernetes][link-2] manifests, then could one use `kustomize` to modify a `kubeadm` configuration file, which is also a YAML manifest? So I asked about it in one of the Kubernetes-related channels in Slack at work, and as it turns out it's not such a crazy idea after all! So, in this post, I'll show you how to use `kustomize` to modify `kubeadm` configuration files.<!--more-->

If you aren't already familiar with `kustomize`, I recommend having a look at [this blog post][xref-1], which provides an overview of this tool. For the base `kubeadm` configuration files to modify, I'll use `kubeadm` configuration files from [this post][xref-2] on setting up a Kubernetes 1.15 cluster with the AWS cloud provider.

While the blog post linked above provides an overview of `kustomize`, it certainly doesn't cover all the functionality `kustomize` provides. In this particular use case---modifying `kubeadm` configuration files---the functionality described in the linked blog post doesn't get you where you need to go. Instead, you'll have to use the _patching_ functionality of `kustomize`, which allows you to overwrite specific fields within the YAML definition for an object with new values.

To do this requires that you provide `kustomize` with enough information to find the specific object you'd like to modify in the YAML resource files. You do this by telling `kustomize` what API group to find, the kind of object to modify, and then uniquely identify the object. For example, in the `kustomization.yaml` file, you'd specify a patch like follows:

```yaml
---
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
- kubeadm.yaml
patches:
- path: cluster-details.yaml
  target:
    group: kubeadm.k8s.io
    version: v1beta2
    kind: ClusterConfiguration
    name: kubeadm-init-clusterconfig
```

Let's break this down just a bit:

* The `resources` field list all the base YAML files that can potentially be modified by `kustomize`. In this case, it points to a single file: `kubeadm.yaml` (which, as you can guess, is a configuration file for `kubeadm`).
* The `path` field for the patch points to a YAML file that contains the fields and values that will be used to modify the base YAML file.
* The `target` fields tell `kustomize` _which_ object in the base YAML file will be modified, by providing `kustomize` with the API group, API version, the kind of object, and the name (as provided by the `metadata.name` field) of the object to be modified.

Put another way, the `resources` field provides the base YAML file(s), the `target` says what object will be changed in the `resources`, and the `path` specifies a file that contains how the `target` in the `resources` will be changed.

(None of this is specific to modifying `kubeadm` configuration files with `kustomize`, by the way---the same fields are needed if you want to patch a Kubernetes object, like a Deployment, Service, or Ingress).

In this case, since you want to modify a `kubeadm` configuration file, the target specifies an API group of "kubeadm.k8s.io", an API version of "v1beta2", an object kind of "ClusterConfiguration", and then a name to unique identify the object (because there may be more than one object of the specified API group, version, and kind in the base YAML files). Wait...a name?

"Scott," you say, "`kubeadm` configuration files don't have a name in them."

Not by default, no---but you can add it, and this is the key to making it possible to use `kustomize` with a `kubeadm` configuration file. For example, here's the `kubeadm` configuration file from the article on setting up Kubernetes with the AWS cloud provider:

```yaml
---
apiVersion: kubeadm.k8s.io/v1beta2
kind: ClusterConfiguration
apiServer:
  extraArgs:
    cloud-provider: aws
clusterName: blogsample
controlPlaneEndpoint: cp-lb.us-west-2.elb.amazonaws.com
controllerManager:
  extraArgs:
    cloud-provider: aws
    configure-cloud-routes: "false"
kubernetesVersion: stable
networking:
  dnsDomain: cluster.local
  podSubnet: 192.168.0.0/16
  serviceSubnet: 10.96.0.0/12
---
apiVersion: kubeadm.k8s.io/v1beta2
kind: InitConfiguration
nodeRegistration:
  kubeletExtraArgs:
    cloud-provider: aws
```

No `metadata.name` field anywhere there, and yet this `kubeadm` configuration file works just fine. Just because the field isn't _required_, though, doesn't mean you can't add it. To make it possible to use `kustomize` with this configuration file, you only need to add a `metadata.name` field to each of the two objects in this file (you _do_ understand there are two objects here, right?), resulting in something like this:

```yaml
---
apiVersion: kubeadm.k8s.io/v1beta2
kind: ClusterConfiguration
metadata:
  name: kubeadm-init-clusterconfig
apiServer:
  extraArgs:
    cloud-provider: aws
clusterName: blogsample
controlPlaneEndpoint: cp-lb.us-west-2.elb.amazonaws.com
controllerManager:
  extraArgs:
    cloud-provider: aws
    configure-cloud-routes: "false"
kubernetesVersion: stable
networking:
  dnsDomain: cluster.local
  podSubnet: 192.168.0.0/16
  serviceSubnet: 10.96.0.0/12
---
apiVersion: kubeadm.k8s.io/v1beta2
kind: InitConfiguration
metadata:
  name: kubeadm-init-initconfig
nodeRegistration:
  kubeletExtraArgs:
    cloud-provider: aws
```

Now each object in this file can be uniquely identified by a `target` field in `kustomization.yaml`, which allows you to modify it using a patch. Handy, right!?

The final piece is the actual patch itself, which is simply a list of the fields to overwrite. As an example, here's a patch that modifies the `clusterName` and `controlPlaneEndpoint` fields:

```yaml
---
apiVersion: kubeadm.k8s.io/v1beta2
kind: ClusterConfiguration
metadata:
  name: kubeadm-init-clusterconfig
clusterName: oregon-cluster-1
controlPlaneEndpoint: different-dns-name.route53-domain.com
```

With all three pieces that are needed---the base YAML file (in this case a `kubeadm` configuration file with the `metadata.name` field added to each object), the `kustomization.yaml` which specifies the patch, and the patch YAML itself---in place, running `kustomize build .` results in this output:

```yaml
apiServer:
  extraArgs:
    cloud-provider: aws
apiVersion: kubeadm.k8s.io/v1beta2
clusterName: oregon-cluster-1
controlPlaneEndpoint: different-dns-name.route53-domain.com
controllerManager:
  extraArgs:
    cloud-provider: aws
    configure-cloud-routes: "false"
kind: ClusterConfiguration
kubernetesVersion: stable
metadata:
  name: kubeadm-init-clusterconfig
networking:
  dnsDomain: cluster.local
  podSubnet: 192.168.0.0/16
  serviceSubnet: 10.96.0.0/12
---
apiVersion: kubeadm.k8s.io/v1beta2
kind: InitConfiguration
metadata:
  name: kubeadm-init-initconfig
nodeRegistration:
  kubeletExtraArgs:
    cloud-provider: aws
```

You'll note that `kustomize` alphabetizes the fields in the base YAML, which is why they are in a different order in this sample output compared to the base YAML. However, you'll also note that the `clusterName` and `controlPlaneEndpoint` fields _were_ modified as instructed. Success!

This is interesting, but why is it important? By using `kustomize` to modify `kubeadm` configuration files, you can declaratively describe the configuration of the cluster you wish to bootstrap with `kubeadm` easily for multiple clusters. In the same way you'd use base resources with overlays for other Kubernetes objects, you can use the same base `kubeadm` files with overlays for different `kubeadm`-bootstrapped clusters. This brings some consistency to how you manage the manifests applied to Kubernetes clusters and the `kubeadm` configuration files used to establish those same clusters. (Oh, and spoiler alert: you can also use `kustomize` on Cluster API manifests! I have a blog post on that coming soon.)

If you have any feedback, questions, corrections (I _am_ human and make mistakes---more frequently than I'd like!), or suggestions for improving this blog post, please don't hesitate to reach out [to me on Twitter][link-3]. I'd be happy to hear from you!

[link-1]: https://github.com/kubernetes-sigs/kustomize/
[link-2]: https://kubernetes.io/
[link-3]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-09-13-an-introduction-to-kustomize.md" >}}
[xref-2]: {{< relref "2019-08-14-setting-up-aws-integrated-kubernetes-115-cluster-kubeadm.md" >}}
