---
author: slowe
categories: Explanation
comments: true
date: 2019-04-16T12:00:00Z
tags:
- AWS
- CLI
- Kubeadm
- Kubernetes
title: Using Kubeadm to Add New Control Plane Nodes with AWS Integration
url: /2019/04/16/using-kubeadm-add-new-control-plane-nodes-aws-integration/
---

In [my recent post][xref-1] on using `kubeadm` to set up a [Kubernetes][link-1] 1.13 cluster with AWS integration, I mentioned that I was still working out the details on enabling AWS integration (via the AWS cloud provider) while also using new functionality in `kubeadm` (specifically, the `--experimental-control-plane` flag) to make it easier to join new control plane nodes to the cluster. In this post, I'll share with you what I've found to make this work.<!--more-->

The challenge here, by the way, is that you can't use the `--config <filename>.yaml` flag _and_ the `--experimental-control-plane` flag at the same time. I did try this, and the results of my testing led me to believe that although `kubeadm` doesn't report an error, it does ignore the `--experimental-control-plane` flag. (Kubernetes experts/contributors, feel free to let me know if I've missed something here.)

After some trial-and-error---mostly my own fault because I didn't take the time to review [the v1beta1 `kubeadm` API docs][link-3] ahead of time---I finally arrived at a working configuration that allows you to use `kubeadm join --config <filename>.yaml` to join a control plane node to an existing AWS-integrated Kubernetes cluster.

Credit for finding the solution goes to Rafael Fernández López, a developer based in Madrid who works for SUSE. Rafael and I had been chatting for a bit on the Kubernetes Slack team, and he pointed me to a particular value he'd been using with `kubeadm` that "flipped the bit," so to speak, in telling `kubeadm` that the node you're joining to the cluster is a control plane node and not a worker node. As proof, he pointed me to [this snippet of code][link-2] that was working for him.

So, based on Rafael's input and my own subsequent testing, I arrived at this YAML configuration file:

``` yaml
---
apiVersion: kubeadm.k8s.io/v1beta1
kind: JoinConfiguration
discovery:
  bootstrapToken:
    token: hjnu4i.ebza7otvg9axuqv7
    apiServerEndpoint: "k8s-8152541997.us-west-1.elb.amazonaws.com:6443"
    unsafeSkipCAVerification: true
nodeRegistration:
  name: ip-172-31-50-70.us-west-2.compute.internal
  kubeletExtraArgs:
    cloud-provider: aws
controlPlane:
  localAPIEndpoint:
    advertiseAddress: 172.31.50.70
```

Naturally, you can't just copy-and-paste this and use it; you'll need to change a few values that are specific to your particular environment/situation:

* The `token` field needs to specify a valid token. Bootstrap tokens generated by `kubeadm init` have a TTL of 24 hours, so you may need to use `kubeadm token create` to create a new token and specify that value here. Try to keep your tokens as short-lived as possible (use the `--ttl` flag to specify a short lifetime), as these are powerful authentication secrets.
* The `apiServerEndpoint` will need to point to the DNS name of the load balancer that sits in front of your control plane.
* You'll have to specify `unsafeSkipCAVerification: true` unless you know the SHA256 hash of the CA certificate. If you _do_ know the SHA256 hash, then you'd replace this line with `caCertHashes: [<hash value>]`. (The brackets are required.)
* The `nodeRegistration.name` value is one I've discussed multiple times; it need to match the EC2 Private DNS entry (and this is also what the OS should have configured for hostname).
* Finally, the `controlPlane.localAPIEndpoint.advertiseAddress` should be the IP address of the instance you're joining to the cluster as a new control plane node. _This is the "magic sauce," so to speak, that tells `kubeadm` you're joining a control plane node to the cluster._

You'd use this configuration file by running `kubeadm join --config <filename>.yaml`. The node should join the cluster as a control plane node, and since the configuration file specifies the AWS cloud provider, you should also get AWS integration.

There you have it---a `kubeadm` configuration file that allows you to use `kubeadm join` to join new control plane nodes to a cluster while enabling the AWS cloud provider.

## Additional Notes

I tested this using Kubernetes 1.14.0, but it should work the same way with Kubernetes 1.13.x as well. I used an external etcd cluster, but it should also work with stacked masters (etcd co-located with the control plane components). Note that I did _not_ test the `--experimental-upload-certs` functionality in Kubernetes 1.14 (it's on my list of things to do).

If you have any questions, comments, suggestions, or corrections, please [contact me on Twitter][link-99]. I'd love to hear from you.

[link-1]: https://kubernetes.io
[link-2]: https://github.com/ereslibre/kubernetes-cluster-vagrant/blob/master/configs/kubeadm-join.config.erb#L16
[link-3]: https://godoc.org/k8s.io/kubernetes/cmd/kubeadm/app/apis/kubeadm/v1beta1
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-02-18-kubernetes-kubeadm-and-the-aws-cloud-provider.md" >}}
