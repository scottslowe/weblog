---
author: slowe
categories: Tutorial
comments: true
date: 2020-12-02T11:00:00-07:00
tags:
- CAPI
- CLI
- Kubernetes
title: Bootstrapping a Cluster API Management Cluster
url: /2020/12/02/bootstrapping-a-cluster-api-management-cluster/
---

[Cluster API][link-2] is, if you're not already familiar, an effort to bring declarative [Kubernetes][link-3]-style APIs to Kubernetes cluster lifecycle management. (I encourage you to check out [my introduction to Cluster API post][xref-1] if you're new to Cluster API.) Given that it is using Kubernetes-style APIs to manage Kubernetes clusters, there must be a _management cluster_ with the Cluster API components installed. But how does one establish that management cluster? This is a question I've seen pop up several times in [the Kubernetes Slack community][link-4]. In this post, I'll walk you through one way of bootstrapping a Cluster API management cluster.<!--more-->

The process I'll describe in this post is also described in the upstream Cluster API documentation (see the "Bootstrap &amp; Pivot" section of [this page][link-1]).

At a high level, the process looks like this:

1. Create a temporary bootstrap cluster.
2. Make the bootstrap cluster into a temporary management cluster.
3. Use the temporary management cluster to establish a workload cluster (through Cluster API).
4. Convert the workload cluster into a permanent management cluster.
5. Remove the temporary bootstrap cluster.

The following sections describe each of these steps in a bit more detail.

## Create a Temporary Bootstrap Cluster

The first step is to create a temporary bootstrap cluster. This will be a short-lived cluster whose only purpose is just to get you to the point of having a more permanent management cluster. There's a few different ways to do this, but probably the _easiest_ way (in my opinion) is to use [`kind`][link-5]. To use `kind`, you'll need to either a) be running on Linux with a container runtime (typically Docker, but Podman is experimentally supported), or b) use something like Docker Desktop for macOS or Windows. The `kind` website also has instructions for [using Windows Subsystem for Linux 2 (WSL2) on Windows 10][link-6].

In order for your `kind` cluster to be able to function as a temporary management cluster, you'll need at least one worker node. The default for `kind` is to only provision a single control plane node, so we'll have to use a custom configuration file (this is [described pretty well on the `kind` website][link-7]). Here's an example configuration file you could use:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
  - role: worker
```

Save this to a file and then reference that file when creating the cluster by running `kind create cluster --config boostrap.yaml` (or whatever you named the file). `kind` will create the cluster and create a Kubeconfig file for accessing the cluster, leaving you ready to proceed directly to the next step. Use `kubectl get nodes` to check the status of the nodes, proceeding to the next step once both nodes report "Ready".

## Establish the Temporary Management Cluster

For this step you'll need `clusterctl`, a Cluster API command-line tool that you'll use extensively through the rest of this process. This tool is available from [the Cluster API GitHub repository][link-8]. Once you've downloaded the tool, marked it as executable (if necessary for your OS), and placed it in your path (again, if necessary for your OS), then you're ready to proceed.

I'll assume you're going to create a management cluster with the AWS provider, so the instructions below will reflect that. If you're going to use a different provider---say, [the Azure provider][link-12] or [the vSphere provider][link-13]---the insructions will look a bit different. Check [the Cluster API Quick Start page][link-9] for examples of how to initialize a management cluster with a particular provider.

For the AWS provider, you'll also need `clusterawsadm` (available from [the Cluster API Provider for AWS GitHub repository][link-10]). I won't walk through all of the steps necessary to prep your AWS environment; [the Quick Start page][link-9] provides a great summary of what's needed.

Once you've prepared your AWS environment (or whatever platform you're going to use), run `clusterctl init --infrastructure aws` to initialize the bootstrap cluster as a Cluster API management cluster. (Replace "aws" with the provider you're using, naturally.) This process normally only takes a few minutes.

Once the process has completed, run `kubectl get clusters` to verify the Cluster API components have been installed. You should get "No resources found in default namespace"; if you see that, you're good to go. If you get an error, then it's time to start troubleshooting.

Once things are working, you're ready to proceed to the next step.

## Create a Workload Cluster

In this step, you'll use the Cluster API components in the bootstrap cluster to create a more long-lived cluster on the infrastructure provider of your choice (whatever provider you installed in the previous section). We refer to this first cluster as a "workload cluster," because it will (initially) be managed by a separate management cluster---the temporary boostrap cluster you've just initialized in the previous section.

To do this, you'll use `clusterctl config cluster` to create a YAML manifest for the workload cluster you'll create. Because each infrastructure provider has different values that are needed in order to create the YAML manifest, I'd recommend use you `clusterctl config cluster --list-variables <provider>`, replacing `<provider>` with the name of the infrastructure provider you're using (like "aws" or "vsphere"). This will give you a list of the values that `clusterctl` expects you to supply in order to generate a complete manifest. For the AWS provider, for example, that command produces this output:

```sh
Variables:
  - AWS_CONTROL_PLANE_MACHINE_TYPE
  - AWS_NODE_MACHINE_TYPE
  - AWS_REGION
  - AWS_SSH_KEY_NAME
  - CLUSTER_NAME
  - CONTROL_PLANE_MACHINE_COUNT
  - KUBERNETES_VERSION
  - WORKER_MACHINE_COUNT
```

This tells me _exactly_ what values I need to supply to `clusterctl`---either as environment variables (what's listed above), as values in a configuration file that I'll then specify with the `--config <file>` parameter, or as command-line parameters to `clusterctl` directly (like the `--kubernetes-version` parameter). I'm a fan of using the configuration file, which would need to look like this (values changed to protect sensitive information):

```yaml
AWS_CONTROL_PLANE_MACHINE_TYPE: m5a.large
AWS_NODE_MACHINE_TYPE: m5a.large
AWS_REGION: us-east-1
AWS_SSH_KEY_NAME: blog_rsa_key
CLUSTER_NAME: demo
CONTROL_PLANE_MACHINE_COUNT: 1
KUBERNETES_VERSION: v1.18.2
WORKER_MACHINE_COUNT: 1
```

With the configuration file in place, you can create the YAML manifest for the workload cluster with `clusterctl config cluster <cluster-name> --config <config-file.yaml>`. This will output to the screen. To actually create this cluster using Cluster API, you have a couple of options:

1. Pipe the output directly to `kubectl` by appending `| kubectl apply -f -` to the command above.
2. Redirect the output to a file by adding `> manifest.yaml` to the command above, and then apply separately with `kubectl apply -f manifest.yaml`.

It will take some time for Cluster API to realize (create) the new cluster. You can use `kubectl get clusters` to check the status of the new cluster. Once the cluster starts reporting as "Provisioned", then you can [retrieve the Kubeconfig for the new cluster][xref-2]. Using the Kubeconfig for the new cluster, install a CNI plugin of your choice (I'm partial to Calico and Antrea). Once `kubectl get nodes` for the new cluster shows the nodes in the newly-created cluster as "Ready", you can proceed to the next step.

## Convert the Workload Cluster to a Management Cluster

Now you're finally ready to move the Cluster API components from the temporary `kind` cluster to the more permanent cluster on the infrastructure provider of your choice (whatever provider you used when you initialized the bootstrap cluster). This process is, in my opinion, pretty straightforward:

1. Set your active Kubeconfig to the temporary management cluster.
2. Run `clusterctl move --to-kubeconfig=<path-to-kubeconfig-for-new-cluster>`. The Kubeconfig being referenced here is the Kubeconfig for the workload cluster you created in the previous section.

Note that this operates on the default namespace; add the `--namespace` flag if you used a different namespace.

After this process completes, you should be able to run `kubectl get clusters` against the new cluster (not the temporary bootstrap cluster) and see the cluster object you defined earlier.

## Decommission the Bootstrap Cluster

At this point, the `kind` bootstrap cluster is no longer needed, so delete it with `kind delete cluster`.

And that's it---you've just bootstrapped a management cluster on the infrastructure provider of your choice! You can now use `clusterctl` to create new workload clusters that will be managed by this management cluster.

## Wrapping Up

I hope this walk-through is helpful. To help make this process even easier, check out [this post on creating a pre-configured machine image][xref-3] (currently only focusing on AWS) to eliminate the need to install `kind`, `clusterctl`, or any of the other utilities.

If you have any questions, please feel free to contact [me on Twitter][link-11], or reach out to me on [the Kubernetes Slack community][link-4]. I'd love to hear from you!

[link-1]: https://cluster-api.sigs.k8s.io/clusterctl/commands/move.html
[link-2]: https://cluster-api.sigs.k8s.io/
[link-3]: https://kubernetes.io/
[link-4]: https://kubernetes.slack.com/
[link-5]: https://kind.sigs.k8s.io/
[link-6]: https://kind.sigs.k8s.io/docs/user/using-wsl2/
[link-7]: https://kind.sigs.k8s.io/docs/user/configuration/
[link-8]: https://github.com/kubernetes-sigs/cluster-api
[link-9]: https://cluster-api.sigs.k8s.io/user/quick-start.html
[link-10]: https://github.com/kubernetes-sigs/cluster-api-provider-aws/
[link-11]: https://twitter.com/scott_lowe
[link-12]: https://github.com/kubernetes-sigs/cluster-api-provider-azure/
[link-13]: https://github.com/kubernetes-sigs/cluster-api-provider-vsphere/
[xref-1]: {{< relref "2019-08-26-an-introduction-to-kubernetes-cluster-api.md" >}}
[xref-2]: {{< relref "2020-02-19-retrieving-kubeconfig-cluster-api-workload-cluster.md" >}}
[xref-3]: {{< relref "2020-06-10-making-it-easier-to-get-started-with-cluster-api-on-aws.md" >}}
