---
author: slowe
categories: Tutorial
comments: true
date: 2021-10-12T12:00:00-06:00
tags:
- AWS
- CLI
- Kubeadm
- Kubernetes
title: Using the External AWS Cloud Provider for Kubernetes
url: /2021/10/12/using-the-external-aws-cloud-provider-for-kubernetes/
---

In 2018, after finding a dearth of information on setting up [Kubernetes][link-3] with AWS integration/support, I set out to try to establish some level of documentation on this topic. That effort resulted in a few different blog posts, but ultimately culminated in [this post on setting up an AWS-integrated Kubernetes cluster using `kubeadm`][xref-1]. Although originally written for Kubernetes 1.15, the process described in that post is still accurate for newer versions of Kubernetes. With the release of Kubernetes 1.22, though, the in-tree AWS cloud provider---which is what is used/described in the post linked above---has been deprecated in favor of [the external cloud provider][link-2]. In this post, I'll show how to set up an AWS-integrated Kubernetes cluster using the external AWS cloud provider.<!--more-->

In addition to the post I linked above, there were a number of other articles I published on this topic:

* [Setting up the AWS Cloud Provider][xref-2]
* [Kubernetes, Kubeadm, and the AWS Cloud Provider][xref-3]
* [Using Kubeadm to Add New Control Plane Nodes with AWS Integration][xref-4]

Most of the information in these posts, if not all of it, is found in [the latest iteration][xref-1], but I wanted to include these links here for some additional context. Also, all of these focus on the now-deprecated in-tree AWS cloud provider.

Although all of these prior posts focus on the in-tree provider, they are helpful because many of the same prerequisites/requirements for the in-tree provider are still---as far as I know---applicable for the external AWS cloud provider:

1. The hostname of each node must match the EC2 Private DNS entry for the instance (by default, this is something like `ip-10-11-12-13.us-west-2.compute.internal` or similar). Note that I haven't explicitly tested/verified this requirement in a while, so it's possible that this has changed. As soon as I am able, I'll conduct some additional testing and update this post.
2. Each node needs to have an IAM instance profile that grants it access to an IAM role and policy with permissions to the AWS API.
3. Specific resources used by the cluster must have certain AWS tags assigned to them. As with the hostname requirement, this is an area where I haven't done extensive testing of the external cloud provider against the in-tree provider.
4. Specific entries are needed in the `kubeadm` configuration file used to bootstrap the cluster, join control plane nodes, and join worker nodes.

The following sections describe each of these four areas in a bit more detail.

## Setting Node Hostnames

Based on my testing---see my disclaimer in #1 above---the hostname for the OS needs to match the EC2 Private DNS entry for that particular instance. By default, this is typically something like `ip-10-11-12-13.us-west-2.compute.internal` (change the numbers and the region to appropriately reflect the private IP address and region of the instance, and be aware that the us-east-1 AWS region uses the `ec2.internal` DNS suffix). The fastest/easiest way I've verified to make sure this is the case is with this command:

    sudo hostnamectl set-hostname \
    $(curl -s http://169.254.169.254/latest/meta-data/local-hostname)

Be sure to set the hostname before starting the bootstrapping process. I have some references of putting this command in the user data for the instance, so that it runs automatically. I have not, however, specifically tested this approach.

## Creating and Assigning the IAM Instance Profile

The nodes in the cluster need permission to the AWS APIs in order for the AWS cloud provider to function properly. The ["Prerequisites" page on the Kubernetes AWS Cloud Provider site][link-4] has a sample policy for both control plane nodes and worker nodes. Consider these sample policies to be starting points; test and modify in order to make sure the policies work for your specific implementation. Once you create IAM instance profiles that reference the appropriate roles and policies, then be sure to specify the IAM instance profile when launching your instances. All the major IaC tools (including both [Pulumi][link-5] and [Terraform][link-6]) have support for specifying the IAM instance profile in code.

## Tagging Cluster Resources

While the documentation for the cloud provider is improving, this is one area that could still use some additional work. The "Getting Started" page on the Kubernetes AWS Cloud Provider site only says this about tags:

> Add the tag `kubernetes.io/cluster/<your_cluster_id>: owned` (if resources are owned and managed by the cluster) or `kubernetes.io/cluster/<your_cluster_id>: shared` (if resources are shared between clusters, and should not be destroyed if the cluster is destroyed) to your instances.

Based on my knowledge of the in-tree provider and the testing I've done with the external provider, this is correct. However, additional tags are typically needed:

* Public (Internet-facing) subnets need a `kubernetes.io/elb: 1` tag, while private subnets need a `kubernetes.io/internal-elb: 1` tag.
* All subnets need the `kubernetes.io/cluster/<your_cluster_id>: owned|shared` tag.
* If the cloud controller manager isn't started with `--configure-cloud-routes: "false"`, then the route tables also needed to be tagged like the subnets.
* At least one security group---one which the nodes should be a member of---needs the `kubernetes.io/cluster/<your_cluster_id>: owned|shared` tag.

Failure to have things properly tagged results in odd failure modes, like ELBs being automatically created in response to the creation of a Service object of type `LoadBalancer`, but instances never being populated for the ELB (for example). Another failure I've seen is the Kubelet failing to start if the nodes aren't properly tagged. Unfortunately, the failure modes of the external cloud provider aren't any better documented than the in-tree provider, which can make troubleshooting a bit challenging.

## Using Kubeadm Configuration Files

The final piece is adding the correct values to your `kubeadm` configuration files so that the cluster is bootstrapped properly. I tested the configurations shown below using Kubernetes 1.22.

Three different configuration files are needed:

1. A configuration file to be used to bootstrap the first control plane node
2. A configuration file used to join any additional control plane nodes
3. A configuration file used to join worker nodes

I'll begin with the natural starting point: the configuration file for bootstrapping the first/initial control plane node.

### Bootstrapping the First Control Plane Node

A `kubeadm` configuration file you could use to bootstrap your first control plane node with the external AWS cloud provider might look something like this:

```yaml
---
apiVersion: kubeadm.k8s.io/v1beta3
kind: ClusterConfiguration
apiServer:
  extraArgs:
    cloud-provider: external
clusterName: foo
controllerManager:
  extraArgs:
    cloud-provider: external
kubernetesVersion: v1.22.2 # can use "stable"
networking:
  dnsDomain: cluster.local
  podSubnet: 192.168.0.0/16
  serviceSubnet: 10.96.0.0/12
---
apiVersion: kubeadm.k8s.io/v1beta3
kind: InitConfiguration
nodeRegistration:
  name: ip-10-11-12-13.us-west-2.compute.internal
  kubeletExtraArgs:
    cloud-provider: external
```

Note that this does not take into account configuration settings related to setting up a highly-available control plane; refer to [the `kubeadm` v1beta3 API docs][link-8] for details on what additional settings are needed. (For sure the `controlPlaneEndpoint` field should be added, but there may be additional settings that are necessary for your specific environment.)

The big change from previous `kubeadm` configurations I've shared is that `cloud-provider: aws` is now `cloud-provider: external`. Otherwise, the configuration remains largely unchanged. Note the absence of the `configure-cloud-routes`; this is moved to the AWS cloud controller manager itself.

After you've bootstrapped the first control plane node (using `kubeadm init --config <filename>.yaml`) but before you add any other nodes---control plane or otherwise---you'll need to install the AWS cloud controller manager. Manifests are available, but you'll need to use `kustomize` to build them out:

    kustomize build 'github.com/kubernetes/cloud-provider-aws/examples/existing-cluster/overlays/superset-role/?ref=master'

Review the output (to ensure the values supplied are correct for your environment), then send the results to your cluster by piping them into `kubectl apply -f -`.

You'll also want to go ahead and install the CNI plugin of your choice.

### Adding More Control Plane Nodes

If you are building a highly-available control plane, then a `kubeadm` configuration similar to the one shown below would work with the external AWS cloud provider:

```yaml
---
apiVersion: kubeadm.k8s.io/v1beta3
kind: JoinConfiguration
discovery:
  bootstrapToken:
    token: 123456.a4v4ii39rupz51j3
    apiServerEndpoint: "cp-lb.us-west-2.elb.amazonaws.com:6443"
    caCertHashes: ["sha256:193feed98fb5fd2b497472fb7d9553414e27ff7eeb7b919c82ff3a08fdf5782f"]
nodeRegistration:
  name: ip-10-14-18-22.us-west-2.compute.internal
  kubeletExtraArgs:
    cloud-provider: external
controlPlane:
  localAPIEndpoint:
    advertiseAddress: 10.14.18.22
  certificateKey: "f6fcb672782d6f0581a1060cf135920acde6736ef12562ddbdc4515d1315b518"
```

You'd want to adjust the values for `token`, `apiServerEndpoint`, `caCertHashes`, and `certificateKey` as appropriate based on the output of `kubeadm init` when bootstrapping the first control plane node. Also, refer to the "Adding More Control Plane Nodes" section of [the previous post][xref-1] for a few notes regarding tokens, the SHA256 hash, and the certificate encryption key (there are ways to recover/recreate this information if you don't have it).

Use your final configuration file with `kubeadm join --config <filename>.yaml` to join the cluster as an additional control plane node.

### Adding Worker Nodes

The final step is to add worker nodes. You'd do this with `kubeadm join --config <filename>.yaml`, where the specified YAML file might look something like this:

```yaml
---
apiVersion: kubeadm.k8s.io/v1beta3
kind: JoinConfiguration
discovery:
  bootstrapToken:
    token: 123456.a4v4ii39rupz51j3
    apiServerEndpoint: "cp-lb.us-west-2.elb.amazonaws.com:6443"
    caCertHashes:
      - "sha256:193feed98fb5fd2b497472fb7d9553414e27ff7eeb7b919c82ff3a08fdf5782f"
nodeRegistration:
  name: ip-10-12-14-16.us-west-2.compute.internal
  kubeletExtraArgs:
    cloud-provider: external
```

As noted earlier, be sure to specify a correct and valid bootstrap token and the SHA256 hash of the CA certificate.

## Wrapping Up

At this point, you should have a (mostly) functional Kubernetes cluster. You'll probably still want some sort of storage solution; see [here][link-1] for more details on the AWS EBS CSI driver.

If you run into problems or issues getting this to work, please feel free to reach out to me. You can find me on [the Kubernetes Slack community][link-9], or you can contact [me on Twitter][link-10] (DMs are open). Also, if you're well-versed in this area and have corrections, clarifications, or suggestions for how I can improve this article, I welcome all constructive feedback. Thanks!

**UPDATE 2021-11-02:** I updated the URL for building the manifests for installing the external AWS cloud provider.

[link-1]: https://github.com/kubernetes-sigs/aws-ebs-csi-driver
[link-2]: https://github.com/kubernetes/cloud-provider-aws
[link-3]: https://kubernetes.io
[link-4]: https://cloud-provider-aws.sigs.k8s.io/prerequisites/
[link-5]: https://www.pulumi.com
[link-6]: https://terraform.io
[link-7]: https://cloud-provider-aws.sigs.k8s.io/getting_started/
[link-8]: https://kubernetes.io/docs/reference/config-api/kubeadm-config.v1beta3/
[link-9]: https://kubernetes.slack.com
[link-10]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-08-14-setting-up-aws-integrated-kubernetes-115-cluster-kubeadm.md" >}}
[xref-2]: {{< relref "2018-09-28-setting-up-the-kubernetes-aws-cloud-provider.md" >}}
[xref-3]: {{< relref "2019-02-18-kubernetes-kubeadm-and-the-aws-cloud-provider.md" >}}
[xref-4]: {{< relref "2019-04-16-using-kubeadm-add-new-control-plane-nodes-aws-integration.md" >}}
