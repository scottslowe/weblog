---
author: slowe
categories: Tutorial
comments: true
date: 2020-02-18T06:00:00-05:00
tags:
- AWS
- CLI
- Kubeadm
- Kubernetes
title: Setting up K8s on AWS with Kubeadm and Manual Certificate Distribution
url: /2020/02/18/setting-up-k8s-on-aws-kubeadm-manual-certificate-distribution/
---

_Credit for this post goes to Christian Del Pino, who created this content and was willing to let me publish it here._

The topic of setting up Kubernetes on AWS (including the use of the AWS cloud provider) is a topic I've tackled a few different times here on this site (see [here][xref-1], [here][xref-2], and [here][xref-3] for other posts on this subject). In this post, I'll share information provided to me by a reader, Christian Del Pino, about setting up Kubernetes on AWS with `kubeadm` but using manual certificate distribution (in other words, not allowing `kubeadm` to distribute certificates among multiple control plane nodes). As I pointed out above, all this content came from Christian Del Pino; I'm merely sharing it here with his permission.<!--more-->

This post specifically builds upon [this post on setting up an AWS-integrated Kubernetes 1.15 cluster][xref-3], but is written for Kubernetes 1.17. As mentioned above, this post will also show you how to manually distribute the certificates that `kubeadm` generates to other control plane nodes.

What this post _won't_ cover are the details on some of the prerequisites for making the AWS cloud provider function properly; specifically, this post won't discuss:

* Setting node hotnames to match EC2's Private DNS entry
* Creating and assigning the IAM instance profile to allow access to the AWS APIs
* Assigning the correct tags to AWS resources as required by the AWS cloud provider

I encourage you to refer to the link above for details on these specific areas.

Now, let's take a look at the process that Christian tested and shared with me.

## Bootstrapping the First Control Plane Node

To build the first control plane node, you need to setup a configuration file that will be used by `kubeadm` to perform an initialization. (Again, this assumes all necessary prerequisites described above have been satisfied.) Here is a sample configuration file:

```yaml
---
apiVersion: kubeadm.k8s.io/v1beta2
kind: ClusterConfiguration
apiServer:
  extraArgs:
    cloud-provider: aws
clusterName:
controlPlaneEndpoint: cp-nlb.elb.us-east-2.amazonaws.com
controllerManager:
  extraArgs:
    cloud-provider: aws
    configure-cloud-routes: "false"
kubernetesVersion: v1.17.0
networking:
  podSubnet: 192.168.0.0/16
---
apiVersion: kubeadm.k8s.io/v1beta2
kind: InitConfiguration
nodeRegistration:
  kubeletExtraArgs:
    cloud-provider: aws
```

The explanation of this configuration file is already pretty well-covered [in this earlier post][xref-3].

Once you have the configuration file set to your liking, run the
following command to setup your first control plane:

    kubeadm init --config=init.yaml

(Replace `init.yaml` with the correct filename, of course.)

Assuming the process of initializing the first control plane node works as expected, then the next step is to install a CNI plugin, as documented [here][link-1].

To confirm that your first control plane is up and ready, just run this command:

    kubectl get nodes

If this command completes successfully and shows the first control plane node as "Ready," then you are now ready to add more control plane nodes. However, since the `--upload-certs` flag was not specified during the `kubeadm init` command, the certificates generated on the first control plane node will not be shared across all the control plane nodes automatically. The next section explains how to manually copy the appropriate files across to additional control plane nodes.

## Manually Distributing Certificates

There are a couple different reasons why a user might prefer to manually distribute certificates. First, the encryption key used by the `--upload-certs` flag expires after two hours. Therefore, it has been more than two hours, you need to _either_ run `kubeadm init phase upload-certs` _or_ manually copy the certificates across to other nodes. Second, you may want to use automation tools to deploy your cluster. One way to copy the certificates is to use `ssh` and `scp`, as described in the `kubeadm` documentation [here][link-2].

You could also use S3 as a means of distributing certificates (more on that shortly).

Regardless of the method, here is the exact list of certificates that need to be copied over from the first control plane node:

    /etc/kubernetes/pki/etcd/ca.key
    /etc/kubernetes/pki/etcd/ca.crt
    /etc/kubernetes/pki/sa.key
    /etc/kubernetes/pki/sa.pub
    /etc/kubernetes/pki/front-proxy-ca.crt
    /etc/kubernetes/pki/front-proxy-ca.key
    /etc/kubernetes/pki/ca.crt
    /etc/kubernetes/pki/ca.key

If you'd like to use `scp`, then simply copy these files across to the EC2 instance(s) you'd like to use as additional control plane nodes.

Since this cluster will be using the AWS cloud provider, EC2 instances have to be booted with an IAM instance profile that enables access to the AWS APIs. If that IAM instance profile includes S3 permissions, and if you can install the AWS CLI tools on the EC2 instances, you can use the `aws s3 cp` command to upload the certificates to a bucket of your choice (just ensure the bucket permissions are appropriately secured). Once the certificates are uploaded to S3 (or copied over), you can proceed to add more control plane nodes.

## Adding Additional Control Plane Nodes

Before adding more control planes, it is necessary to pull down the
certificates from the S3 bucket using the `aws s3 cp` command into the
/etc/kubernetes/pki directory (if you didn't copy them across directly using `scp`). You must keep the same structure as in the first control plane:

    /etc/kubernetes/pki/etcd/ca.key
    /etc/kubernetes/pki/etcd/ca.crt
    /etc/kubernetes/pki/sa.key
    /etc/kubernetes/pki/sa.pub
    /etc/kubernetes/pki/front-proxy-ca.crt
    /etc/kubernetes/pki/front-proxy-ca.key
    /etc/kubernetes/pki/ca.crt
    /etc/kubernetes/pki/ca.key

Once the certificates are in place, you can build the configuration
file for your joining control planes:

```yaml
---
apiVersion: kubeadm.k8s.io/v1beta2
kind: JoinConfiguration
discovery:
  bootstrapToken:
    token: 123456.i2v5ub39hrvf51j3
    apiServerEndpoint: cp-nlb.elb.us-east-2.amazonaws.com
    caCertHashes:
      - "sha256:082feed98fb5fd2b497472fb7d9553414e27ff7eeb7b919c82ff3a08fdf5782f"
nodeRegistration:
  name: ip-10-10-100-155.us-east-2.compute.internal
  kubeletExtraArgs:
    cloud-provider: aws
controlPlane:
  localAPIEndpoint:
    advertiseAddress: 10.10.100.155
```

Refer back to [the earlier post][xref-3] for information on the parameters in this configuration file. The key difference here is that the configuration file does not include the `certificateKey` parameter since the original `kubeadm init` command did not include the `--upload-certs` flag.

Once the file is complete, proceed to run the following to join your
additional control plane nodes:

    kubeadm join --config=cp-join.yaml

(Replace `cp-join.yaml` with the correct filename you used.)

Once run, the new control plane node should join the cluster. You can confirm that the additional control planes have joined the cluster by running `kubectl get nodes` and make sure the additional nodes are reporting as "Ready".

## Adding Worker Nodes

Adding worker nodes is simple. There are no certificates to copy and the
`kubeadm` configuration to join the cluster is similar to that of the
joining control plane nodes. Here is a sample:

```yaml
---
apiVersion: kubeadm.k8s.io/v1beta2
kind: JoinConfiguration
discovery:
  bootstrapToken:
    token: 123456.i2v5ub39hrvf51j3
    apiServerEndpoint: "cp-nlb.elb.us-east-2.amazonaws.com"
    caCertHashes:
      - "sha256:082feed98fb5fd2b497472fb7d9553414e27ff7eeb7b919c82ff3a08fdf5782f"
nodeRegistration:
  name: ip-10-10-100-133.us-east-2.compute.internal
  kubeletExtraArgs:
    cloud-provider: aws
```

As outlined here, you'll need to be sure your version of this file uses the correct bootstrap token, the correct API server endpoint, the correct CA certificate hash, and the correct node name. Once the file is complete, you will run the following to join your worker nodes:

    kubeadm join --config=worker-join.yaml

(Substitute the filename you used for `worker-join.yaml` in the above command.)

Upon running this command, the worker node should join the cluster. Once again, you can use `kubectl get nodes` to verify that the node(s) have joined the cluster.

## Summary

As you can see above, the process that Christian shared with me is extremely similar to what I shared [here][xref-3], with the exception of manually distributing the certificates (and Christian mentions a couple very good reasons above why manual certificate distribution may be preferred). This post also shows that the process for establishing an AWS-integrated Kubernetes cluster is consistent for versions 1.15, 1.16, and 1.17 (you just need to change the `kubernetesVersion` value in the configuration file you use to bootstrap the first control plane node).

I hope this information is helpful. Thanks to Christian for sharing this information with me and allowing me to publish it here. If you have any questions, feel free to [hit me on Twitter][link-3] or find me on [the Kubernetes Slack instance][link-4].

[link-1]: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#pod-network
[link-2]: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/#manual-certs
[link-3]: https://twitter.com/scott_lowe
[link-4]: https://kubernetes.slack.com
[xref-1]: {{< relref "2018-09-28-setting-up-the-kubernetes-aws-cloud-provider.md" >}}
[xref-2]: {{< relref "2019-02-18-kubernetes-kubeadm-and-the-aws-cloud-provider.md" >}}
[xref-3]: {{< relref "2019-08-14-setting-up-aws-integrated-kubernetes-115-cluster-kubeadm.md" >}}
