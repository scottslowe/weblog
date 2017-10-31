---
author: slowe
categories: Liveblog
comments: true
date: 2017-10-18T15:00:00Z
tags:
- Docker
- DockerCon2017
- OSS
title: "Looking Under the Hood: containerD"
url: /2017/10/18/looking-under-hood-containerd/
---

This is a liveblog of the session titled "Looking Under the Hood: containerD", presented by Scott Coulton with Puppet (and also a Docker Captain). It's part of the Edge track here at DockerCon EU 2017, where I'm attending and liveblogging as many sessions as I'm able.<!--more-->

Coulton starts out by explaining the session (it will focus a bit more on how to consume containerD in your own software projects), and provides a brief background on himself. Then he reviews the agenda, and dives right into the content.

Up first, Coulton starts by providing a bit of explanation around what containerD is and does. He notes that there is a CLI tool for containerD (the `ctr` tool), and that containerD uses a gRPC API listening on a local UNIX socket. Coulton also discusses `ctr`, but points out that `ctr` is, currently, an unstable tool (it is changing quickly). Next, Coulton talks about how containerD provides support for the OCI Image Spec and the OCI Runtime Spec (of which `runC` is an implementation), image push/pull support, and management of namespaces.

Coulton moves into a demo showing off some of containerD's functionality, using the `ctr` tool.

After the demo, Coulton talks about some other upstream projects that use containerD. Those projects include Moby, cri-containerD; he notes that containerD itself can leverage OCI-compliant runtimes (that adhere to the OCI Runtime Spec). Mostly containerD is used in Moby, LinuxKit, and Kubernetes.

This leads Coulton into a discussion of the Moby Project, which is the upstream open source project that then feeds into the commercial products Docker CE and Docker EE. Various components of Moby include SwarmKit, HyperKit, InfraKit, LinuxKit, containerD, and runC.

Coulton also takes a moment to explain what's in Docker but isn't in containerD. As examples, containerD won't _build_ images, and it lacks any of the access controls or content security features that Docker may have. To help illustrate some of these differences, Coulton shows a demo of Docker running inside a container managed by containerD.

Switching gears a bit now, Coulton changes topics to focus on LinuxKit. LinuxKit is a lean, composable OS where everything runs as a container. Currently, LinuxKit is running kernel 4.9, with newer versions listed as "experimental." LinuxKit supports any container runtime, which means you could use containerD/runC. LinuxKit has some advantages over "traditional" OSes in that it is smaller, with a smaller attack surface, and designed specifically for use with immutable infrastructure patterns. By default, a LinuxKit image contains an init image, containers for containerD and runC, and some CA certificates---that's all.

This brings us to a demo by Coulton of building a LinuxKit image. Because LinuxKit is so small and builds quickly, it's a pretty short demo. To build a LinuxKit image, you'd use the `moby` tool and a YAML file that specifies what should be included in the LinuxKit image.

Moving on from LinuxKit, Coulton now turns to Kubernetes and containerD. In order for containerD to interact with Kubernetes, the Kubelet talks to a CRI (Container Runtime Interface) shim via gRPC, and the shim in turn talks to a container runtime (such as containerD). The CRI shim needed in the case of containerD is [cri-containerd][link-1].

At this point, Coulton wraps up his content and opens up the session to questions from the audience.



[link-1]: https://github.com/kubernetes-incubator/cri-containerd
