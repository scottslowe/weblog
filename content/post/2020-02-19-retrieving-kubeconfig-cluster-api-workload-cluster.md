---
author: slowe
categories: Explanation
comments: true
date: 2020-02-19T16:30:00-07:00
tags:
- Kubernetes
- CAPI
- CLI
title: Retrieving the Kubeconfig for a Cluster API Workload Cluster
url: /2020/02/19/retrieving-kubeconfig-cluster-api-workload-cluster/
---

Using [Cluster API][link-2] allows users to create new [Kubernetes][link-1] clusters easily using manifests that define the desired state of the new cluster (also referred to as a workload cluster; see [here][xref-1] for more terminology). But how does one go about accessing this new workload cluster once it's up and running? In this post, I'll show you how to retrieve the Kubeconfig file for a new workload cluster created by Cluster API.<!--more-->

(By the way, there's nothing new or revolutionary about the information in this post; I'm simply sharing it to help folks who may be new to Cluster API.)

Once CAPI has created the new workload cluster, it will also create a new Kubeconfig for accessing this cluster. This Kubeconfig will be stored as a Secret (a specific type of Kubernetes object). In order to use this Kubeconfig to access your new workload cluster, you'll need to retrieve the Secret, decode it, and then use it with `kubectl`.

First, you can see the secret by running `kubectl get secrets` in the namespace where your new workload cluster was created. If the name of your workload cluster was "workload-cluster-1", then the name of the Secret created by Cluster API would be "workload-cluster-1-kubeconfig".

To retrieve the Secret and decode it, use this command:

    kubectl get secret <cluster-name>-kubeconfig -o jsonpath='{.data.value}' | base64 -D > /path/to/saved/kubeconfig/file

Let me break down this command a bit. First, you're using the `-o jsonpath` parameter to the `kubectl get secret` command to show _only_ the value of the Secret (and not include all the associated metadata). Second, the command pipes this value through to `base64`, which uses the `-D` switch to decode the base64-encoded contents of the Secret. (This command is for macOS; the command will be slightly different on Linux or Windows.) Finally, you are redirecting the output of the command into a file.

Once you have the Kubeconfig file decoded and saved locally, then you can use it to access the workload cluster like this:

    export KUBECONFIG=/path/to/saved/kubeconfig/file
    kubectl cluster-info

Note that context---the user and cluster that `kubectl` will communicate with---is _really_ important when working with multiple clusters, like in a Cluster API environment. I **very strongly recommend** using a tool that will show you your current Kubernetes context on your prompt, so that you know which cluster will receive the commands you're typing. Otherwise, you could end up deploying workloads to your management cluster or trying to send Cluster API manifests to a workload cluster. [`powerline-go`][link-3] is a great option, in my opinion.

You should now be all set to access your new Cluster API-managed workload cluster. If you have questions, feel free to reach out [to me on Twitter][link-5], or contact me on [the Kubernetes Slack instance][link-4]. Enjoy!

[link-1]: https://kubernetes.io/
[link-2]: https://github.com/kubernetes-sigs/cluster-api
[link-3]: https://github.com/justjanne/powerline-go
[link-4]: https://kubernetes.slack.com
[link-5]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-08-26-an-introduction-to-kubernetes-cluster-api.md" >}}
