---
author: slowe
categories: Tutorial
comments: true
date: 2020-03-19T13:15:00-07:00
tags:
- Kubernetes
- Docker
- macOS
title: Using KinD with Docker Machine on macOS
url: /2020/03/19/using-kind-with-docker-machine-on-macos/
---

I'll admit right up front that this post is more "science experiment" than practical, everyday use case. It all started when I was trying some [Cluster API][link-6]-related stuff that leveraged [KinD (Kubernetes in Docker)][link-7]. Obviously, given the name, KinD relies on [Docker][link-8], and when running Docker on macOS you generally would use Docker Desktop. At the time, though, I was using Docker Machine, and as it turns out KinD doesn't like Docker Machine. In this post, I'll show you how to make KinD work with Docker Machine.<!--more-->

By the way, it's worth noting that, per the KinD maintainers, [this isn't a tested configuration][link-2]. Proceed at your own risk, and know that while this may work for some use cases it won't necessarily work for _all_ use cases.

## Prerequisites/Assumptions

These instructions assume you've already installed both KinD and Docker Machine, along with an associated virtualization solution. I'll be using [VirtualBox][link-3], but this should be largely the same for [VMware Fusion][link-4] or [Parallels][link-5] (or even HyperKit, if you somehow manage to get that working). I'm also assuming that you have `jq` installed; if not, get it [here][link-9].

## Making KinD work with Docker Machine

Follow the steps below to make KinD work with Docker Machine. In all the commands below, replace `vm` with whatever name you're using, and remember to specify your virtualization driver (using `-d`) where necessary.

1. Start your Docker Machine VM using `docker-machine start vm`.
2. Set your Docker host to the Docker Machine VM using `eval $(docker-machine env vm)`.
3. Get the IP address of your Docker Machine VM using `docker-machine inspect vm | jq -r '.Driver.IPAddress'`. Make a note of this IP address; you'll need it shortly.
4. Create a configuration file for KinD with the following contents, and make a note of the name (I'll assume you called it `kind.yaml`). Replace `<insert-ip-address-here>` with the IP address of your Docker Machine VM, obtained in the previous step.

        ---
        apiVersion: kind.x-k8s.io/v1alpha4
        kind: Cluster
        networking:
          apiServerAddress: "<insert-ip-address-here>"

5. Run `kind create cluster --config kind.yaml` to stand up a Kubernetes cluster using the Docker daemon running in your Docker Machine VM. If you used a filename _other_ than `kind.yaml` for the file generated in the previous step, substitute that filename in the `kind create cluster` command.

With the added configuration file specified in step 5, KinD will now generate a configuration that will work with Docker Machine. Once step 5 finishes, you should be able to use `kubectl` to interact with the local Kubernetes cluster created by KinD. Things like port forwarding and such may behave differently (I haven't had time to extensively test and document the findings), so keep that in mind.

All things considered, though, it's probably easier to just use Docker Desktop.

## Additional Resources

I found [this blog post][link-1] by teammate John Harris to be enormously helpful, especially in my testing when I thought I'd have to use `kubeadmConfigPatches` to add a SAN to the API server's certificate. (It turns out it isn't needed.)

Contact [me on Twitter][link-10] or find me on [the Kubernetes Slack instance][link-11] if you have questions, corrections, suggestions for improvement, or if you just want to chat. Thanks!

[link-1]: https://johnharris.io/2019/04/kubernetes-in-docker-kind-of-a-big-deal/
[link-2]: https://github.com/kubernetes-sigs/kind/issues/916
[link-3]: https://www.virtualbox.org/
[link-4]: https://www.vmware.com/products/personal-desktop-virtualization.html
[link-5]: https://www.parallels.com/
[link-6]: https://github.com/kubernetes-sigs/cluster-api
[link-7]: https://kind.sigs.k8s.io/
[link-8]: https://www.docker.com/
[link-9]: https://stedolan.github.io/jq/
[link-10]: https://twitter.com/scott_lowe
[link-11]: https://kubernetes.slack.com/
