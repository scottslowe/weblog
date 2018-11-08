---
author: slowe
categories: Tutorial
comments: true
date: 2018-09-28T12:00:00Z
tags:
- AWS
- Kubernetes
- Networking
- Storage
title: Setting up the Kubernetes AWS Cloud Provider
url: /2018/09/28/setting-up-the-kubernetes-aws-cloud-provider/
---

The AWS cloud provider for [Kubernetes][link-5] enables a couple of key integration points for Kubernetes running on AWS; namely, dynamic provisioning of Elastic Block Store (EBS) volumes and dynamic provisioning/configuration of Elastic Load Balancers (ELBs) for exposing Kubernetes Service objects. Unfortunately, the documentation surrounding how to set up the AWS cloud provider with Kubernetes is woefully inadequate. This article is an attempt to help address that shortcoming.<!--more-->

More details are provided below, but at a high-level here's what you'll need to make the AWS cloud provider in Kubernetes work:

* The hostname of each node must match EC2's private DNS entry for that node
* An IAM role and policy that EC2 instances can assume as an instance profile
* Kubernetes-specific tags applied to the AWS resources used by the cluster
* Particular command-line flags added to the Kubernetes API server, Kubernetes controller manager, and the Kubelet

Let's dig into these requirements in a bit more detail.

## Node Hostname

It's important that the name of the Node object in Kubernetes matches the private DNS entry for the instance in EC2. You can use `hostnamectl` or a confiugration management tool (take your pick) to set the instance's hostname to the FQDN that matches the EC2 private DNS entry. This typically looks something like `ip-10-15-30-45.us-west-1.compute.internal`, where `10-15-30-45` is the private IP address and `us-west-1` is the region where the instance was launched.

If you're unsure what it is, or if you're looking for a programmatic way to retrieve the FQDN, just `curl` the AWS metadata server:

    curl http://169.254.169.254/latest/meta-data/local-hostname

Make sure you set the hostname before attempting to bootstrap the Kubernetes cluster, or you'll end up with nodes whose name in Kubernetes doesn't match up, and you'll see various "permission denied"/"unable to enumerate" errors in the logs. For what it's worth, preliminary testing indicates that this step---setting the hostname to the FQDN---is necessary for Ubuntu but may not be needed for CentOS/RHEL.

## IAM Role and Policy

Because the AWS cloud provider performs some tasks on behalf of the operator---like creating an ELB or an EBS volume---the instances need IAM permissions to perform these tasks. Thus, you need to have an IAM instance profile assigned to the instances that gives them permissions.

The _exact_ permissions that are needed haven't been documented anywhere that I've found (please contact me if you have more information), but here's a summary of permissions for control plane nodes that works:

* Allow permissions on `ec2:*`
* Allow permissions on `elasticloadbalancing:*`
* Allow permissions on `ecr:GetAuthorizationToken`, `ecr:BatchCheckLayerAvailability`, `ecr:GetDownloadUrlForLayer`, `ecr:GetRepositoryPolicy`, `ecr:DescribeRepositories`, `ecr:ListImages`, and `ecr:BatchGetImage`
* Allow permissions on `autoscaling:DescribeAutoScalingGroup` and `autoscaling:UpdateAutoScalingGroup`

It's probably possible to whittle down the `ec2:*` permission for control plane nodes. Some preliminary thoughts that my colleague Joe Beda shared with me indicates that the following permissions may be enough: `ec2:DescribeInstances`, `ec2:DescribeSecurityGroups`, `ec2:AttachVolume`, `ec2:DetachVolume`, `ec2:DescribeVolumes`, `ec2:CreateVolume`, `ec2:DeleteVolume`, `ec2:DescribeSubnets`, `ec2:CreateSecurityGroup`, `ec2:DeleteSecurityGroup`, `ec2:AuthorizeSecurityGroupIngress`, `ec2:RevokeSecurityGroupIngress`, `ec2:CreateTags`, `ec2:DescribeRouteTables`, `ec2:CreateRoute`, `ec2:DeleteRoute`, and `ec2:ModifyInstanceAttribute`. Note, however, that this list has **not** been tested or validated.

For worker nodes, the permissions are not as far-reaching:

* Allow permission on `ec2:Describe*`
* Allow permissions on `ecr:GetAuthorizationToken`, `ecr:BatchCheckLayerAvailability`, `ecr:GetDownloadUrlForLayer`, `ecr:GetRepositoryPolicy`, `ecr:DescribeRepositories`, `ecr:ListImages`, and `ecr:BatchGetImage`

You'll want to capture these permissions in a policy statement, associate that policy statement with a role, and then associate that role with an IAM instance profile. You'll end up with two IAM instance profiles: one for the control plane nodes with a broader set of permissions, and one for the worker nodes with a more restrictive set of permissions.

(UPDATE: Reader Matt Roux pointed out that [the `cloud-provider-aws` GitHub repository][link-6]---which represents the future of the AWS cloud provider---contains a proper IAM policy for control plane nodes and worker nodes. Thanks Matt!)

## AWS Tags

The AWS cloud provider needs a specific tag to be present on almost all the AWS resources that a Kubernetes cluster needs. The tag key is `kubernetes.io/cluster/<cluster-name>`; the value of the tag is immaterial (this tag replaces an older `KubernetesCluster` tag). Note that Kubernetes itself will also use this tag on things that it creates, and it will use a value of "owned". This value does _not_ need to be used on resources that Kubernetes itself did not create. Most of the documentation I've seen indicates that the tag is needed on all instances and on exactly one security group (this is the security group that will be modified to allow ELBs to access the nodes, so the worker nodes should be a part of this security group). However, I've also found it necessary to make sure the `kubernetes.io/cluster/<cluster-name>` tag is present on subnets and route tables in order for the integration to work as expected.

## Kubernetes Configuration

On the Kubernetes side of the house, you'll need to make sure that the `--cloud-provider=aws` command-line flag is present for the API server, controller manager, and every Kubelet in the cluster.

If you're using `kubeadm` to set up your cluster, you can have `kubeadm` add the flags to the API server and controller manager by using the "apiServerExtraArgs" and "controllerManagerExtraArgs" sections in a configuration file, like this:

``` yaml
apiServerExtraArgs:
  cloud-provider: aws
controllerManagerExtraArgs:
  cloud-provider: aws
```

Likewise, you can use the "nodeRegistration" section of a `kubeadm` configuration file to pass extra arguments to the Kubelet, like this:

``` yaml
nodeRegistration:
  kubeletExtraArgs:
    cloud-provider: aws
```

I'd probably also recommend setting the name of the Kubelet to the node's private DNS entry in EC2 (this ensures it matches the hostname, as described earlier in this article). Thus, the full "nodeRegistration" section might look like this:

``` yaml
nodeRegistration:
  name: ip-10-15-30-45.us-west-1.compute.internal
  kubeletExtraArgs:
    cloud-provider: aws
```

You would need to substitute the correct fully-qualified domain name for each instance, of course.

Finally, for dynamic provisioning of Persistent Volumes you'll need to create a default Storage Class. The AWS cloud provider has one, but it doesn't get created automatically. Use this command to define the default Storage Class:

    kubectl apply -f https://raw.githubusercontent.com/kubernetes/kubernetes/master/cluster/addons/storage-class/aws/default.yaml

This will create a Storage Class named "gp2" that has the necessary annotation to make it the default Storage Class (see [here][link-1]). Once this Storage Class is defined, dynamic provisioning of Persistent Volumes should work as expected.

## Troubleshooting

Troubleshooting is notoriously difficult, as most errors seem to be "transparently swallowed" instead of exposed to the user/operator. Here are a few notes that may be helpful:

* You _must_ have the `--cloud-provider=aws` flag added to the Kubelet **before** adding the node to the cluster. Key to the AWS integration is a particular field on the Node object---the `.spec.providerID` field---and that field will _only_ get populated if the flag is present when the node is added to the cluster. If you add a node to the cluster and then add the command-line flag afterward, this field/value won't get populated and the integration won't work as expected. No error is surfaced in this situation (at least, not that I've been able to find).
* If you do find yourself with a missing `.spec.providerID` field on the Node object, you can add it with a `kubectl edit node` command. The format of the value for this field is `aws:///<az-of-instance>/<instance-id>`.
* Missing AWS tags on resources will cause odd behaviors, like failing to create an ELB for a LoadBalancer-type Service. I haven't had time to test all the different failure scenarios, but if the cloud provider integration isn't working as expected I'd double-check that the Kubernetes-specific tags are present on all the AWS resources.

Hopefully the information in this article helps remove some of the confusion and lack of clarity around getting the AWS cloud provider working with your Kubernetes cluster. I intend to keep this document updated as I discover additional failure scenarios or find more detailed documentation. If you have questions, feel free to [hit me on Twitter][link-3] or find me in [the Kubernetes Slack community][link-4]. (If you're an expert in the AWS cloud provider code and can help flesh out the details of this post, please contact me as well!) Have fun out there, fellow Kubernauts!

[link-1]: https://kubernetes.io/docs/tasks/administer-cluster/change-default-storage-class/
[link-2]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[link-3]: https://twitter.com/scott_lowe
[link-4]: https://kubernetes.slack.com
[link-5]: https://kubernetes.io
[link-6]: https://github.com/kubernetes/cloud-provider-aws
