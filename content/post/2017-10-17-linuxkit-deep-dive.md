---
author: slowe
categories: Liveblog
comments: true
date: 2017-10-17T14:00:00Z
tags:
- Docker
- DockerCon2017
- Linux
title: LinuxKit Deep Dive
url: /2017/10/17/linuxkit-deep-dive/
---

This is a liveblog of the DockerCon EU session titled "LinuxKit Deep Dive". The speakers are Justin Cormack and Rolf Neugebauer, both with Docker, and this session is part of the "Black Belt" track here at DockerCon.<!--more-->

So what is LinuxKit? It's a toolkit, part of the Moby Project, that is used for building secure, portable, and lean operating systems for containers. It uses the `moby` tooling to build system images. LinuxKit uses YAML files to describe the complete system, and these files are consumed by `moby` to assemble the boot image and verify the signature. On top of that is containerD, which runs on-boot containers, service containers, and shutdown containers. Think of on-boot and shutdown containers as one-time containers that perform some task, either when the system is booting or shutting down (respectively).

LinuxKit was first announced and open sourced in April 2017 at DockerCon in Austin. Major additions since it was announced include:

* arm64 support
* Improved Kubernetes support
* Linux containers on Windows (LCOW) preview support
* Improved platform support (Azure, packet.net, IBM Bluemix, AWS, GCP, VMware, Hyper-V, etc.)
* Multi-arch build system
* Fully immutable system images
* Namespace sharing
* Support for the latest Linux kernels

After reviewing the changes and growth of LinuxKit since its announcement, Neugebauer launches into a demo of LinuxKit. The demo shows using LinuxKit to build an identical image across three different architectures (x86, ARM64, and Raspberry Pi).

LinuxKit images start with an Alpine image that mirrors all the Alpine `apk` packages needed to build LinuxKit packages. Because all the packages are in the image, it can be built without network access and enable repeatable builds. Then, leveraging multi-stage builds, only the specific needed packages are copied over to the scratch-based secondary image build. This results in very small packages (less than 2 MB in size).

Neugebauer continues to show how multi-architecture LinuxKit builds are handled using `docker build`, `manifest-tool` (with a shout-out to Phil Estes), and `notary`.

Cormack now takes over to talk about some networking functionality in LinuxKit. Wireguard was something that was integrated in LinuxKit recently (it's still be upstreamed in the Linux kernel). As an example, Cormack shows how he would use Wireguard and LinuxKit to build an encrypted tunnel from a Redis container to another container. It looks useful, but the demo feels rushed and doesn't do a very good job of showing how someone might use this functionality yourself.

Shifting topics, Cormack talks about how Docker uses LinuxKit to build Kubernetes (primarily used today in Docker for Mac/Windows, where Docker is adding Kubernetes support in the next release). There is a (too) brief demo of this as well, but there is a fair amount of hand-waving over some of the details and explanations of the various pieces involved.

At this point, Cormack reviews some additional resources on LinuxKit and then opens the session up to questions from the audience.
