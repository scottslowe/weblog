---
author: slowe
categories: Tutorial
comments: true
date: 2019-08-27T17:00:00Z
tags:
- Kubernetes
- CLI
- CAPI
- AWS
title: Bootstrapping a Kubernetes Cluster on AWS with Cluster API
url: /2019/08/27/bootstrapping-a-kubernetes-cluster-on-aws-with-clusterapi/
---

Yesterday I published [a high-level overview of Cluster API][xref-1] (CAPI) that provides an introduction to some of the concepts and terminology in CAPI. In this post, I'd like to walk readers through actually using CAPI to bootstrap a [Kubernetes][link-3] cluster on AWS. This walkthrough is for the v1alpha1 release of CAPI (a walk through for CAPI v1alpha2 is coming).<!--more-->

It's important to note that all of the information shared here is also found in [the "Getting Started" guide][link-2] in [the AWS provider's GitHub repository][link-4]. My purpose here is provide an additional walkthrough that supplements that official documentation, not to supplant the official documentation, and to spread the word about how the process works. As mentioned earlier, this is all based on the 0.3.x release of the AWS provider, which adheres to the v1alpha1 revision of CAPI.

Four basic steps are involved in bootstrapping a Kubernetes cluster on AWS using CAPI:

1. Installing the necessary tools (a one-time task)
2. Preparing the AWS account with the correct IAM roles and policies (this is a one-time task)
3. Creating a management cluster (not required every single time)
4. Creating a workload cluster

The following sections take a look at each of these steps in a bit more detail. First, though, I think it's important to mention that CAPI is still in its early days (this post is based on v1alpha1). As such, it's possible that commands may (will) change, and API specifications may (will) change as further development occurs. In fact, there are changes in the process from v1alpha1 (which is what this post is based upon) and the v1alpha2 release.

With that caveat/warning provided, let's dig into the details.

## Installing the Necessary Tools

There are (generally) three tools you'll want to install on your local system:

1. `kind` (available [via GitHub][link-5])
2. `clusterawsadm` and `clusterctl` for the CAPI AWS provider (available [here][link-4]; this post was written using the 0.3.7 release)
3. `kubectl` (see [instructions here][link-6] for installing)

You'll (generally) also want to have the AWS CLI installed and configured, although this isn't a hard requirement.

Once all the necessary tools are installed, users can proceed with the next step of preparing their AWS account for CAPA.

## Preparing the AWS Account

Before users can use CAPI to stand up clusters in AWS, first the AWS account has to be prepared with an appropriate set of IAM roles and policies. These IAM entities are needed in order to allow the bootstrap and management clusters to create resources on AWS (the bootstrap cluster will create resources consumed by the management cluster; the management cluster will create resources consumed by the workload clusters).

The Cluster API AWS Provider (referred to as CAPA) provides a binary tool that helps with this task, named `clusterawsadm`.

To prepare the account, you'd run `clusterawsadm alpha bootstrap create-stack`. This does assume that you have either a) an AWS CLI profile configured that sets AWS region and credentials, or b) set the appropriate environment variables (`AWS_REGION`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and `AWS_SESSION_TOKEN` if you are using multi-factor authentication). With one of those two conditions satisfied, `clusterawsadm` will generate a CloudFormation stack that creates all the necessary IAM roles and policies.

If you're interested in having a deeper understanding of exactly what `clusterawsadm` is doing, you can instead run `clusterawsadm alpha bootstrap generate-cloudformation <aws-account-id>` (it appears that this portion of functionality from `clusterawsadm` does _not_ respect/recognize AWS CLI profiles). This command will generate a YAML template for CloudFormation that, if applied, would create all the necessary IAM roles and policies.

Users _can_ create the roles and policies manually, but this isn't generally recommended.

## Creating a Management Cluster

To create _any_ cluster using CAPA---even the management cluster---users first have to create the YAML manifests that describe the desired state of the cluster they're creating. To make this process a little easier, the CAPA maintainers have created some templates and a shell script to build out manifests. In [the "Getting Started" guide][link-2] in the GitHub repository, the steps under "Generating cluster manifests and example cluster" walk users through downloading these templates and the associated shell script (appropriately named `generate-yaml.sh`).

**Before** users run `generate-yaml.sh` to create manifests for your management cluster, they can make life a bit easier for themselves by first declaring some environment variables. The shell script uses these environment variables to customize the manifests. Here are some variables to declare before running `generate-yaml.sh`:

* `CLUSTER_NAME` is the name users want to assign to the cluster (this must be unique within an AWS region)
* `AWS_REGION` is, naturally, the AWS region in which the cluster should be created
* `SSH_KEY_NAME` is the name of the AWS SSH keypair in the target region that users want injected into the EC2 instances (for SSH access)
* `CONTROL_PLANE_MACHINE_TYPE` specifies the instance type used for the control plane nodes
* `NODE_MACHINE_TYPE` provides the instance type used for the worker nodes in the cluster

Once these variables are defined, running `generate-yaml.sh` will generate a customized set of manifests into an `out` subdirectory of the current directory. In that `out` directory you'll find a number of files:

* `cluster.yaml` contains the definition of the cluster that will be created. This includes things like CIDR ranges and DNS domain.
* `controlplane-machine.yaml` contains a specification for a non-redundant (single node) control plane.
* `controlplane-machines-ha.yaml` contains a specification for an redundant (multi-node) control plane. This isn't necessarily truly HA (highly available), though, as all the nodes are deployed into the _same_ AWS AZ.
* `machine-deployment.yaml` is for a MachineDeployment (a mechanism for running multiple nodes) for worker nodes.
* `machines.yaml` contains specifications for worker nodes.
* `provider-components.yaml` contains the CRDs for CAPI, as well as a Kubernetes Secret that contains the AWS credentials. (As such, treat this file with extreme care/caution.)
* `addons.yaml` contains specifications for installing the CNI plugin (Calico by default).

If needed, users can further customize these files (like changing the version of Kubernetes they're deploying, for example, but be aware that only specific versions are supported with CAPI).

Users also need to be aware that they don't need _all_ these files (users can just delete the one they don't need):

* For the control plane, users should use **either** `controlplane-machine.yaml` or `controlplane-machines-ha.yaml`.
* For the worker nodes, users should (at first) use **either** `machine-deployment.yaml` or `machines.yaml`.

Once the manifests are ready (they've been appropriately customized and unnecessary files removed), users are ready to create the management cluster with this command:

```shell
clusterctl create cluster -v 3 \
--bootstrap-type kind \
--provider aws \
-c ./out/cluster.yaml \
-m ./out/machines.yaml \
-p ./out/provider-components.yaml \
-a ./out/addons.yaml
```

This will take a fair amount of time to complete (I haven't timed it exactly, but I'd say between 10-15 minutes or so). If you want more verbose output, increase the value of the `-v` parameter. Once it is finished, `clusterctl` will generate a Kubeconfig file in the current directory file---surprise, surprise---`kubeconfig`. Using `kubectl --kubeconfig ./kubeconfig get nodes` should report back on the status of the cluster. (I prefer to copy the file into `~/.kube` and use `ktx` to switch between clusters.) Be aware that `clusterctl` will report "complete" _before_ all the nodes in the cluster are finished provisioning.

Congratulations! You've established a management cluster, which is now prepared with CAPI components to provision and manage workload clusters. From this point on---aside from creating additional management clusters---users will use `kubectl` to interact with CAPI. If you need to provision additional management clusters, you would use this same process with `clusterctl`.

## Creating a Workload Cluster

Once a management cluster has been established, users can use CAPI via that management cluster to instantiate one or more workload clusters. Overall, the process for creating a workload cluster is _very_ simple once you have a management cluster up and running:

1. Generate the YAML manifests that describe the workload cluster you want instantiated.
2. Use `kubectl` to apply those manifests to the management cluster.

Users can use the same `generate-yaml.sh` script and associated environment variables, or users can copy the YAML files for the management cluster and edit them manually. (I'm working on a post about using `kustomize` to help with this process.)

Once the manifests are ready, users just use `kubectl` like this (users should substitute the correct filename and path in the commands below, of course):

1. To create the cluster structure and configuration, run `kubectl apply -f cluster.yaml`.
2. To create the control plane, run `kubectl apply -f controlplane.yaml`.
3. To install a CNI, follow the instructions for that particular CNI.
4. To create the worker nodes, run `kubectl apply -f machines.yaml`.

At each step, CAPA (the AWS provider for CAPI) will create and configure the required AWS resources (VPCs, subnets, load balancers, instances, security groups, etc.). Once the process is finished, there will be a Kubeconfig file named `kubeconfig` in the current directory that users can use to access this new workload cluster. Remember that the steps above should be performed against the **management cluster**. This does seem a bit odd, but just remember that it is the CAPI controllers in the management cluster that will create the resources and elements for the workload cluster.

And that's it---you've created a management cluster on AWS, and then used that management cluster to bootstrap a Kubernetes workload cluster on AWS. You can now use that management cluster to stamp out as many additional workload clusters as needed. The nice thing about using CAPA is that it handles all the AWS-specific details needed to get the AWS cloud provider working (all the stuff described [here][xref-2]).

Got questions, comments, or corrections? I'd love to hear them. [Hit me up on Twitter][link-99] and let me know. Looking for a walk through using v1alpha2 (a newer release) of CAPI? It will be available soon.

**UPDATE 13 September 2019:** I've updated this post to make it clearer it is based on CAPI v1alpha1, and that separate content will be generated for CAPI v1alpha2.

[link-2]: https://github.com/kubernetes-sigs/cluster-api-provider-aws/blob/v0.3.7/docs/getting-started.md
[link-3]: https://kubernetes.io/
[link-4]: https://github.com/kubernetes-sigs/cluster-api-provider-aws/
[link-5]: https://github.com/kubernetes-sigs/kind/
[link-6]: https://kubernetes.io/docs/tasks/tools/install-kubectl/
[link-99]: https://twitter.com/scott_lowe/
[xref-1]: {{< relref "2019-08-26-an-introduction-to-kubernetes-cluster-api.md" >}}
[xref-2]: {{< relref "2019-08-14-setting-up-aws-integrated-kubernetes-115-cluster-kubeadm.md" >}}
