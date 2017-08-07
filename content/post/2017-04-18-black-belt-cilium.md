---
author: slowe
categories: Liveblog
comments: true
date: 2017-04-18T00:00:00Z
tags:
- Networking
- Docker
- DockerCon2017
- Cilium
- Linux
title: 'Liveblog: Cilium for Network and Application Security with BPF and XDP'
url: /2017/04/18/black-belt-cilium/
---

This is a liveblog of the DockerCon 2017 Black Belt session led by Thomas Graf on [Cilium][link-1], a new startup that focuses on using eBPF and XDP for network and application security.<!--more-->

Graf starts by talking about how BPF (specifically, extended BPF or eBPF) can be used to rethink how the Linux kernel handles network traffic. Graf points out that there is another session by Brendan Gregg on using BPF to do analysis performance and profiling.

Why is it necessary to rethink how networking and security is handled? A lot of it has not evolved as application deployments have evolved from low complexity/low deployment frequency to high complexity/high deployment frequency. Further, the age of unique protocol ports (like SMTP on port 25 or SSH on port 22) is coming to a close, as now many different applications or services simply run over HTTP. This leads to "overloading" the HTTP port and a loss of visibility into _which_ applications are talking over that port. Opening TCP port 80 in a situation like this means potentially exposing more privileges than desired (the example to use other HTTP verbs, like PUT or POST instead of just GET).

Graf quickly moves into a (scripted) demo that shows off how you can use Cilium---which leverages eBPF to enforce HTTP-level security policies---to control the types of access that containers have when communicating with other containers via HTTP.

Following the demo, Graf explains how Cilium (and BPF) works. Inbound HTTP requests are rerouted, using BPF, into a transparent proxy. Using a BPF map, the shared state (the origin IP and identity) are preserved, so that the proxy can appropriately redirect the traffic into the destination container. The proxy leverages a set of rules, which allows the proxy to reject the request (sending back an HTTP 403 error code) or forward the request to the destination container. While you can write BPF bytecode directly, it may be easier/simpler to write C pseudocode and compile that into BPF using LLVM. The BPF bytecode is then run through a verifier and just-in-time (JIT) compiler to convert the bytecode into native CPU instructions. The verifier ensures that BPF bytecode is safe to run in the kernel (this helps ensure that BPF bytecode doesn't compromise the stability of the kernel). The compiled BPF bytecode then can "hook" various actions and points in the kernel (to redirect traffic into a transparent proxy, for example).

The use of BPF allows Cilium to "rethink" how policy enforcement happens. BPF can reject unauthorized connections _before_ the Linux kernel even starts building TCP packets and moving them among various components within the kernel.

Shifting focus slightly, Graf moves on to discuss XDP/BPF and the "software load balancer of the future." BPF/XDP allows for a 10x improvement in load balancing over IPVS for L3/L4 traffic. What is XDP? To understand XDP, you have to look at how BPF works. Normally, BPF works in the kernel, meaning traffic coming in on a NIC has to be copied into memory so BPF can operate on that traffic. XDP moves BPF _onto the NIC itself_ XDP runs in the NIC driver, allowing it immediate access to data being received so that policy can be enforced immediately on the NIC. (This is pretty powerful, IMHO.)

At this point Graf starts talking more directly about Cilium, the project with which Graf is affiliated that leverages BPF/XDP. Cilium uses a libnetwork plugin to integrate into Docker. Using a Cilium agent on the host, Cilium generates a BPF program every time a Docker container is launched, and this BPF program is attached to the Docker container. Every Docker container gets its own BPF program. To connect the containers together, Cilium connects the container-specific programs to a BPF program on the NIC. You manage Cilium via a CLI, allowing you to monitor activity and apply/enforce policy.

Graf provides an update on the Cilium project. It's early yet, but there is a Vagrant environment that allows you to see how Cilium works. Graf indicates that Cilium will offer a YAML file for Moby that will allow users to leverage LinuxKit to test Cilium.

Graf wraps up the session by reiterating the need for HTTP-aware network and security functions, and reiterating his believe that BPF/XDP are the vehicles that will drive this functionality in the Linux kernel moving forward. At this point, Graf opens up for questions from session attendees.


[link-1]: http://cilium.io/
