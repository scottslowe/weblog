---
author: slowe
categories: Education
comments: true
date: 2020-11-30T12:00:00-07:00
tags:
- Kubernetes
- Docker
- Linux
title: Assigning Node Labels During Kubernetes Cluster Bootstrapping
url: /2020/11/30/assigning-node-labels-during-kubernetes-cluster-bootstrapping/
---

Given that [Kubernetes][link-1] is a primary focus of my day-to-day work, I spend a fair amount of time in [the Kubernetes Slack community][link-2], trying to answer questions from users and generally be helpful. Recently, someone asked about assigning node labels while bootstrapping a cluster with `kubeadm`. I answered the question, but afterward started thinking that it might be a good idea to also share that same information via a blog post---my thinking being that others who also had the same question aren't likely to be able to find my answer on Slack, but would be more likely to find a published blog post. So, in this post, I'll show how to assign node labels while bootstrapping a Kubernetes cluster.<!--more-->

The "TL;DR" is that you can use the `kubeletExtraArgs` field in a `kubeadm` configuration file to pass the `node-labels` command to the Kubelet, which would allow you to assign node labels when `kubeadm` bootstraps the node. Read on for more details.

## Testing with Kind

`kind` is a great tool for testing this sort of configuration, since `kind` uses `kubeadm` to bootstrap its nodes. If you aren't familiar with `kind`, I encourage you to visit [the `kind` website][link-4]; in particular, check out [the "Configuration" page][link-5], which provides examples of how to use a configuration file to customize how `kind` works.

The "Kubeadm Config Patches" section actually provides the answer to the original question---how does one assign node labels during the bootstrapping process---in one of its examples (the examples shows the use of `kubeletExtraArgs` to assign node labels).

To test it, you could create a YAML file (I called mine `kind-node-labels.yaml`) with the following contents:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
name: label-test
nodes:
  - role: control-plane
  - role: worker
    kubeadmConfigPatches:
      - |
        kind: JoinConfiguration
        nodeRegistration:
          kubeletExtraArgs:
            node-labels: "org.scottlowe.workload-type/pci-dss=true"
```

(The full reference for the configuration file API is available [here][link-3] and is another excellent resource for working with `kind`.)

This configuration file specifies a single control plane and a single worker, and uses the `kubeadmConfigPatches` section to show the use of a `JoinConfiguration` (which is what worker nodes would use when joining a cluster) to assign the node label "org.scottlowe.workload-type/pci-dss=true". This is the sort of label one might use to help influence workload placement only on specified nodes.

To test this, just run `kind create cluster --config kind-node-labels.yaml` (or whatever filename you used). It will take a few minutes for `kind` to do its thing, but when its done you can run `kubectl get nodes --show-labels` and you should see your new node label(s) present.

Use `kind destroy cluster --name label-test` (or whatever name you specified in the configuration file) to destroy the cluster, update the node labels being assigned, and test again until you're happy with the result.

## Using in a Real Environment

Once you've used `kind` to verify the configuration you want to use, then you're ready to use it in a real environment with `kubeadm` and an associated configuration file.

Because `kind` uses `kubeadm`, then the changes you tested above are largely lifted intact from the `kind` configuration file and put into a `kubeadm` configuration file. For example, after you've already bootstrapped a control plane node using `kubeadm init`, then you could use this configuration file to join a worker node and assign a node label at the same time:

```yaml
apiVersion: kubeadm.k8s.io/v1beta2
kind: JoinConfiguration
discovery:
  bootstrapToken:
    token: vulq9h.gu12345678906l74
    apiServerEndpoint: "control-plane-elb.us-west-2.elb.amazonaws.com:6443"
    caCertHashes: ["sha256:123456789012345678901234567890564b934be406f13e28f118b32cc0b6e6db"]
nodeRegistration:
  name: ip-10-11-12-13.us-west-2.compute.internal
  kubeletExtraArgs:
    cloud-provider: aws
    node-labels: "org.scottlowe.workload-type/pci-dss=true"
```

As you can see in the above example, you can combine arguments for `kubeletExtraArgs`; the example above shows enabling the in-tree AWS cloud provider _and_ assigning a node label.

Join the worker node to the cluster using `kubeadm join --config kubeadm.yaml` (or whatever you named the file), and after it's joined the cluster you should again be able to use `kubectl get nodes --show-labels` to see the new node and its node labels.

## Additional Information

Keep in mind that, per [this page on the `kubelet` command line reference][link-7], assigning node labels is an alpha feature. Also, there some restrictions on what labels can and can't be assigned. For example, if you want to assign something in the `kubernetes.io` namespace, there are only certain labels that are permitted and the rest are restricted. You can't, for example, assign the `node-role.kubernetes.io` label (it's restricted). (Hat tip to both Duffie Cooley and Lubomir Ivanov for pointing out these restrictions.)

I hope this information is helpful to folks. If you have any questions, you're welcome to contact [me on Twitter][link-6] or find me on [the Kubernetes Slack community][link-2]. I'm happy to help if I'm able.

[link-1]: https://kubernetes.io/
[link-2]: https://kubernetes.slack.com/
[link-3]: https://godoc.org/sigs.k8s.io/kind/pkg/apis/config/v1alpha4
[link-4]: https://kind.sigs.k8s.io/
[link-5]: https://kind.sigs.k8s.io/docs/user/configuration/
[link-6]: https://twitter.com/scott_lowe
[link-7]: https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/
