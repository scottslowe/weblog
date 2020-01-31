---
author: slowe
categories: Education
comments: true
date: 2018-06-05T12:00:00Z
tags:
- Kubernetes
- CLI
- Linux
title: "Exploring Kubernetes with Kubeadm, Part 1: Introduction"
url: /2018/06/05/exploring-kubernetes-kubeadm-part-1-introduction/
---

I recently started using `kubeadm` more extensively than I had in the past to serve as the primary tool by which I stand up Kubernetes clusters. As part of this process, I also discovered the `kubeadm alpha phase` subcommand, which exposes different sections (phases) of the process that `kubeadm init` follows when bootstrapping a cluster. In this blog post, I'd like to kick off a series of posts that explore how one could use the `kubeadm alpha phase` command to better understand the different components within Kubernetes, the relationships between components, and some of the configuration items involved.<!--more-->

Before I go any further, I'd like to point readers to [this URL][link-1] that provides an overview of `kubeadm` and using it to bootstrap a cluster. If you're new to `kubeadm`, go read that before continuing on here.

&lt;aside&gt;Quick side note: it's my understanding that at some point the intent is to move `kubeadm alpha phase` out of alpha, at which point the command might look more like `kubeadm phase` or similar (that hasn't been fully determined yet as far as I know). If you're reading this at some point in the future, just make note that this was written back in the Kubernetes 1.10 timeframe when this was still an alpha feature.&lt;/aside&gt;

Done? OK, let's proceed. At this point, you should know that `kubeadm init` is a simple and pretty straightforward way to set up a minimally viable Kubernetes cluster. This is all well and good if that is your goal. If, on the other hand, your goal is to better understand Kubernetes, then this is where I recommend spending some time with the `kubeadm alpha phase` subcommands. Why? Instead of automating the entire process, these subcommands let you look at the individual steps _and_ the configuration artifacts produced by these individual steps. Want to see how the API server is configured? You can see the static Pod manifest that `kubeadm` generates. Want to see how certificates are used by Kubernetes to secure communications between the different components? This is visible via the `kubeadm alpha phase` subcommands. In my opinion, this makes `kubeadm alpha phase` a valuable learning tool for those seeking to deepen their knowledge of Kubernetes.

Currently, `kubeadm` is only available for Linux. However, even if you're working on a Linux-based desktop (as I am, for example---I run [Fedora][link-4] 27 as my primary OS), the best place to experiment with `kubeadm alpha phase` is in a Linux VM. Why? Some of the paths where `kubeadm` will output files (certificates, Pod manifests, etc.) are hard-coded within `kubeadm` itself, and you may not have/want to create that directory structure on your Linux desktop. Fortunately, it's easy to spin up a Linux VM using any number of tools ([Vagrant][link-3] comes to mind for me), and installing `kubeadm` is also straightforward (see [these instructions][link-2] for more details).

In future parts of this series, I'll dig into specific sections of the `kubeadm alpha phase` subcommands and review what the subcommand is doing, what some of the configuration artifacts are, and explore how this fits into Kubernetes as a whole. I'll update this post with links to subsequent posts as the series evolves.

Along the way, if you find _anything_ incorrect---I'm human and will make mistakes---feel free to [hit me up on Twitter][link-5] or submit a pull request fixing the error. This will help make this resource more useful for everyone over time.

**UPDATE 2020-01-31:** As of the Kubernetes 1.13 release, `kubeadm alpha phase` was replaced with `kubeadm init phase`, so adjust the commands above accordingly.

[link-1]: https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/
[link-2]: https://kubernetes.io/docs/tasks/tools/install-kubeadm/
[link-3]: https://www.vagrantup.com/
[link-4]: https://getfedora.org/
[link-5]: https://twitter.com/scott_lowe/
