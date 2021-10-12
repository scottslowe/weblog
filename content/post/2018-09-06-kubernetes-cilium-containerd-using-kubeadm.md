---
author: slowe
categories: Tutorial
comments: true
date: 2018-09-06T15:00:00Z
tags:
- Cilium
- CLI
- Kubeadm
- Kubernetes
- Networking
title: Kubernetes with Cilium and Containerd using Kubeadm
url: /2018/09/06/kubernetes-cilium-containerd-using-kubeadm/
---

Now, if that isn't a title jam-packed with buzzwords, I don't know what is! In seriousness, though, I wanted to share how to use `kubeadm` to turn up a [Kubernetes][link-4] cluster using [containerd][link-3] (instead of Docker) and [Cilium][link-2] as the CNI plugin. I'm posting this because I wasn't able to find a reasonable article that combined _all_ the different threads---some posts talked about using containerd, others talked about using Cilium, and the official Kubernetes docs have examples for using `kubeadm`. The purpose of this post is to try to pull those threads together.<!--more-->

For structure and context, I'll build upon the official Kubernetes document outlining [creating highly available clusters with `kubeadm`][link-1]. You may find it helpful to pull up that article next to this one, as I won't be duplicating that content here. Instead, I'll just reference additions/changes to the process in order to accommodate containerd and Cilium.

Before getting started, make sure that your systems will meet the [minimum requirements for Cilium][link-8]. For my testing, I used Ubuntu 16.04 with the latest HWE kernel (4.15.0-33-generic). I used a private fork of [Wardroom][link-7] to build the AWS AMIs with containerd and all the Kubernetes 1.11.2 packages installed, and wrote a custom Ansible script to update the AMIs with the latest HWE kernel. (Side note: I do hope to get my containerd-related changes into Wardroom at some point in the future, so others will be able to leverage Wardroom as well.) Obviously, you can choose to manually build your own images; see the "Additional Resources" section for a link on how to install containerd. You'll want to do that before trying to turn up the Kubernetes cluster.

Ready? Let's go! I'll organize the content below according to the section headings from the official Kubernetes procedure for [creating highly available clusters using `kubeadm`][link-1].

## Bootstrap the First Stacked Control Plane Node

Up until this point, everything remains the same (no changes are needed to accommodate an alternate container runtime or a specific CNI plugin). Once you get to this point and you're ready to bootstrap the first control plane node, some additional changes are needed. Specifically, you need to add the following content to the `kubeadm` configuration file:

``` yaml
nodeRegistration:
  criSocket: /var/run/containerd/containerd.sock
```

This assumes, naturally, that you're using the default location for the runtime endpoint. Modify as needed.

I also had to do the following on my Ubuntu 16.04 nodes, but I take this as more of an issue with my images (you may not need these commands):

    modprobe br_netfilter
    sysctl -w net.ipv4.ip_forward=1

With this content added, you're good to go to run `kubeadm init --config <config-file-name>` to bootstrap the first master. `kubectl get nodes` will report it as NotReady until you install the CNI, but that's OK. You can proceed with the next steps in the procedure.

## Add the Second Stacked Control Plane Node

The only change here is to (again) add this content to the `kubeadm` config file:

``` yaml
nodeRegistration:
  criSocket: /var/run/containerd/containerd.sock
```

You may also need to run the `modprobe` and `sysctl` commands outlined in the previous section. I suspect the need to run those commands reflects a problem in my custom AMIs.

Run the rest of the commands in this section as outlined, and your second control plane node should come online.

## Add the Third Stacked Control Plane Node

The same change to the `kubeadm` configuration file is needed here, along with (maybe) the `modprobe` and `sysctl` commands.

Once this step is complete, you'll have a three-node control plane up and running, although they'll all report NotReady in `kubectl get nodes` until you install a networking plugin. Fortunately, you'll tackle that next.

## Install a Pod Network

Now you'll start getting into some Cilium-specific stuff. For the most part, this is pretty straightforward. You can follow the instructions found [here][link-10], with a few changes. I recommend downloading the Cilium-CRIO YAML file instead of the one linked in the instructions (you can find it [here][link-11]). Then follow the instructions for configuring the ConfigMap for etcd and creating the TLS secrets.

However, before proceeding with deploying Cilium, there are a few additional changes to make to the YAML file (these notes assume you are using v1.2 of the Cilium-CRIO YAML file):

* On line 120, change `--container-runtime=crio` to `--container-runtime=containerd`.
* Add a line immediately after line 120 with the contents `--container-runtime-endpoint=containerd=/var/run/containerd/containerd.sock` (follow the formatting/indentation of the previous line). Adjust the location of the runtime endpoint as needed.
* Change line 221 from `crio-socket` to `containerd-socket`.
* Change line 222 to `/var/run/containerd/containerd.sock`.
* Change line 249 from `crio-socket` to `containerd-socket`.
* Change line 251 to `/var/run/containerd/containerd.sock`.

Once you have these changes in place, you can proceed with the instructions in the "Deploying" section of the [standard install guide][link-10].

Assuming that Cilium deploys properly, your control plane nodes should switch from NotReady to Ready in `kubectl get nodes`, and you should see some Cilium pods scheduled and running when you run `kubectl -n kube-system get pods`.

## Install Workers

Once the control plane nodes are reporting Ready (and perhaps you've done some troubleshooting with Cilium to ensure that it's working as expected), then you're ready to add worker nodes.

When you run `kubeadm init...` on the first stacked control plane node, it will spit out a command line you can use to join worker nodes to the Kubernetes cluster. You can use this command, but you must add one small piece. At the end of the command, add `--cri-socket /var/run/containerd/containerd.sock`. This tells `kubeadm` that this node is using containerd as the container runtime, and will enable the join operation to succeed.

## Additional Resources

The information is this post was based on a number of other articles along with my direct hands-on experience. Readers may want to review these other documents for additional information:

* [Cilium, Kubernetes, and CRI-O][link-5]: Although this document is focused on CRI-O, comparing the YAML manifests referenced by this document with the YAML manifests compared in the standard install guide (found [here][link-10]) helped me understand the differences that would be necessary for containerd.
* The [command reference for `cilium-agent`][link-6]
* The [Ansible+`kubeadm` instructions in the containerd GitHub repository][link-9] provided insight on how to install containerd, although the `kubeadm` instructions aren't complete enough (need to be combined with the official Kubernetes docs as I describe above).

I hope this information is useful. If you have questions (or corrections!), please [hit me up on Twitter][link-12]. Thanks!

[link-1]: https://kubernetes.io/docs/setup/independent/high-availability/
[link-2]: https://cilium.io/
[link-3]: https://github.com/containerd/containerd
[link-4]: https://kubernetes.io/
[link-5]: https://cilium.readthedocs.io/en/v1.2/gettingstarted/cilium_install_crio/
[link-6]: https://cilium.readthedocs.io/en/v1.2/cmdref/cilium-agent/
[link-7]: https://github.com/heptiolabs/wardroom
[link-8]: https://cilium.readthedocs.io/en/v1.2/install/system_requirements/
[link-9]: https://github.com/containerd/cri/tree/master/contrib/ansible
[link-10]: https://cilium.readthedocs.io/en/v1.2/kubernetes/install/standard/
[link-11]: https://github.com/cilium/cilium/blob/v1.2/examples/kubernetes/1.11/cilium-crio.yaml
[link-12]: https://twitter.com/scott_lowe
