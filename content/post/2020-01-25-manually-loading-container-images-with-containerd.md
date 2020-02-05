---
author: slowe
categories: Tutorial
comments: true
date: 2020-01-25T09:55:00-07:00
tags:
- Linux
- AWS
- CLI
- containerD
- Kubernetes
title: Manually Loading Container Images with containerD
url: /2020/01/25/manually-loading-container-images-with-containerd/
---

I recently had a need to manually load some container images into a Linux system running [`containerd`][link-3] (instead of [Docker][link-1]) as the container runtime. I say "manually load some images" because this system was isolated from the Internet, and so simply running a container and having `containerd` automatically pull the image from an image registry wasn't going to work. The process for working around the lack of Internet access isn't difficult, but didn't seem to be documented anywhere that I could readily find using a general web search. I thought publishing it here may help individuals seeking this information in the future.<!--more-->

For an administrator/operations-minded user, the primary means of interacting with `containerd` is via the `ctr` command-line tool. This tool uses a command syntax very similar to Docker, so users familiar with Docker should be able to be productive with `ctr` pretty easily.

In my specific example, I had a bastion host with Internet access, and a couple of hosts behind the bastion that did not have Internet access. It was the hosts behind the bastion that needed the container images preloaded. So, I used the `ctr` tool to fetch and prepare the images on the bastion, then transferred the images to the isolated systems and loaded them. Here's the process I followed:

1. On the bastion host, first I downloaded (pulled) the image from a public registry using `ctr image pull` (the example I'll use here is for the Calico node container image, used by the Calico CNI in Kubernetes clusters):

        ctr image pull docker.io/calico/node:v3.11.2

    (Note that `sudo` may be needed for all these `ctr` commands; that will depend on your system configuration.)

    I repeated this process for all the other images the isolated systems also needed.

    If you have a system (like your local laptop) running Docker, then you can use `docker pull` here instead; just note that you may need to adjust the path/URL to the image/image registry.

2. Still on the bastion host, I exported the pulled images to standalone archives:

        ctr image export calico-node-v3.11.2.tar \
        docker.io/calico/node:v3.11.2

    The general format for this command looks like this:

        ctr image export <output-filename> <image-name>

    If you don't know what the image name (according to `containerd`) is, use `ctr image ls`.

    If you're using a system with Docker installed (maybe you're using your local laptop), then `docker save <image-name> -o <output-filename` will get you an image you can use in the subsequent steps.

3. After transferring the standalone archives to the other systems (using whatever means you prefer; I used `scp`), then load (or import) the images into `containerd` with this command:

        ctr image import <filename-from-previous-step>

    Repeat as needed for additional images. It appears, by the way, that using wildcards in the `ctr image import` command won't work; I had to manually specify each individual file for import.

    _If you need these images to be available to [Kubernetes][link-9]_, you **must** be sure to add the `-n=k8s.io` flag to the `ctr image import` command, like this:

        ctr -n=k8s.io images import <filename-from-previous-step>

4. Verify that the image(s) are present and recognized by `containerd` using `ctr image ls`.
    
    If you specified the `k8s.io` namespace when importing the images in the previous step---so as to make the images available to Kubernetes---then you can verify that CRI (Container Runtime Interface, the means by which Kubernetes talks to `containerd`) sees these images by running `crictl images` (again, `sudo` may be required, based on your configuration).

That should do it for you!

## How I Tested

To test this procedure, I used multiple Ubuntu 18.04 instances on AWS. One instance had Internet access (the bastion host); the others did not. The instances had been built from AMIs prepared using [Packer][link-7] and [Ansible][link-6] with playbooks from [the Kubernetes "image-builder" repository][link-5] (specifically, the playbooks used to build the Kubernetes Cluster API images), so `containerd` and all the other CLI tools mentioned in this article were already installed and available for use.

## Additional Resources

I did find a few sites with some `containerd` information while searching, and wanted to include them here for credit and additional context:

[Getting started with Containerd][link-2]

[`crictl` User Guide][link-4]

I hope the information in this post is helpful. If you have questions, comments, suggestions, or corrections, you are welcome to [contact me on Twitter][link-8], or drop me an e-mail (my e-mail address isn't too hard to find).

**UPDATE:** I've added a few Docker commands to steps #1 and #2 above for users who may be using a Docker-powered system, like their local laptop, to retrieve images from the Internet that will be transferred to the `containerd`-powered systems for importing.

[link-1]: https://www.docker.com/
[link-2]: https://sweetcode.io/getting-started-with-containerd/
[link-3]: https://containerd.io
[link-4]: https://github.com/containerd/cri/blob/master/docs/crictl.md
[link-5]: https://github.com/kubernetes-sigs/image-builder
[link-6]: https://www.ansible.com/
[link-7]: https://packer.io/
[link-8]: https://twitter.com/scott_lowe
[link-9]: https://kubernetes.io
