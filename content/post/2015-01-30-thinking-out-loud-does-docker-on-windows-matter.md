---
author: slowe
categories: Musing
comments: true
date: 2015-01-30T11:20:00Z
tags:
- ToL
- Windows
- Microsoft
- Docker
- Linux
- OSS
- Virtualization
title: 'Thinking Out Loud: Does Docker on Windows Matter?'
url: /2015/01/30/thinking-out-loud-does-docker-on-windows-matter/
---

Nigel Poulton recently posted [an article][link-2] titled "ESXi vs. Hyper-V - Could Docker Support Be Significant," in which he contemplates whether Microsoft's announcement of Docker support on Windows will be a factor in the hypervisor battle between ESXi and Hyper-V. His post got me thinking---which is a good thing, I'd say---and I wanted to share my thoughts on the topic here.

Naturally, it's worth pointing out that I work for VMware, although I do work in a business unit that makes a multi-hypervisor product (NSX).

Nigel makes a few key points in his article:

* Open source is "where it's at today"
* Everyone "is crazy with container fever"
* VMware "ditched" Linux in the transition to ESXi
* Microsoft's support for Docker means Microsoft might "ship a hypervisor platform (Windows Server/Hyper-V) that does VMs and Docker containers"
* Azure could be made more relevant in the public cloud race through "native support for Docker containers" resulting in "native Type-1 hypervisor, native Docker containers."

To be completely fair, the article fully admits that all this is assumption and is just thinking out loud (his statement, not a play on the title of this post). As I said, I think it's a good thing to prompt readers to start thinking and come to conclusions of their own.

Here's my take on it. First, it's important to understand that, as [I mentioned on Twitter][link-3], Docker on Windows isn't the same as Linux workloads on Windows. In other words, having Docker on Windows doesn't mean you can run Linux workloads on Windows. If open source is "where it's at" (to borrow Nigel's phrase, and I don't necessarily disagree), then that means people are going to want to use Linux workloads. And if Docker on Windows doesn't mean Linux workloads on Windows, then why use Docker on Windows? Why not stick with Docker on Linux?

Second, if Docker on Windows doesn't mean Linux workloads on Windows, then it must mean Windows workloads on Windows. The problem, though, as Nick Weaver [mentioned on Twitter][link-1], is that the challenge is wrapping commands into an image. How does one go about describing a Windows application in such a way that a full-featured Docker daemon could package that application into an image? Is it even possible?

Third, the real value of Docker is in the workflow---the ability for a developer to create a simple set of instructions that allows him or her to quickly and easily build an image that remains relatively portable across systems (although the level of that portability is still the subject of some debate). At their core, Linux and Windows are quite different, and this is reflected in a number of ways. Is it even possible for the Docker DSL (the Dockerfile) to describe a Windows application? Or would it have to be extended for Windows applications, thus creating a "Docker DSL for Linux" and a "Docker DSL for Windows"? Doesn't this then diminish the value of the Docker workflow, when there are different commands for building Linux images vs. building Windows images? What about file system layers? Will they work the same on both Linux and Windows?

Clearly, there are a **lot** of technical details that still have to be determined, so---as Nigel stated in his post---a lot of this is conjecture and assumption.

My point, though, is this: Docker support on Windows (which, as far as we know, means a Docker daemon on Windows not the ability to run Linux workloads on Windows) won't necessarily benefit from a "halo effect" due to the popularity of open source. Therefore, it seems to me that "Docker support" on Hyper-V won't be a significant deciding factor in the ongoing battle of the hypervisors. I think that VMware and Microsoft have equal footing in the challenge to provide support for "native Linux containers" on their respective platforms, because it's native Linux containers---managed/coordinated/orchestrated via Docker and related tools and platforms---that seem to be the focus of everyone's attention at the moment. Whichever company does the best job of providing seamless support and outstanding performance for native Linux containers will, I think, gain the upper hand.

Of course, these are my thoughts only.


[link-1]: https://twitter.com/lynxbat/status/561203439917494272
[link-2]: http://blog.nigelpoulton.com/esxi-vs-hyper-v-could-docker-support-be-significant/
[link-3]: https://twitter.com/scott_lowe/status/561216003506311168
