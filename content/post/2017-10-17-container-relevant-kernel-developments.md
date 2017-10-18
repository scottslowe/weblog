---
author: slowe
categories: Liveblog
comments: true
date: 2017-10-17T15:00:00Z
tags:
- Docker
- DockerCon2017
- Linux
title: Container-Relevant Kernel Developments
url: /2017/10/17/container-relevant-kernel-developments/
---

This is a liveblog of a Black Belt track session at DockerCon EU in Copenhagen. The session is named "Container-Relevant Kernel Developments," and the presenter is Tycho Andersen.<!--more-->

Andersen first presents a disclaimer that the presentation is mostly a brain dump, and the he's not personally responsible for a lot of the work presented here. In fact, all of the work Andersen will talk about is not yet merged upstream in the Linux kernel, and he doesn't expect that they will be accepted upstream and see availability for average users.

The first technology Andersen talks about IMA (Integrity Management Association, I think?), which prevents user space from even opening files if they have been tampered with or modified in some fashion that violates policy. IMA is also responsible for allowing the Linux kernel to take advantage of a system's Trusted Platform Module (TPM).

Pertinent to containers, Andersen talks about work that's happening within the kernel development community around namespacing IMA. There are a number of challenges here, not all of which have been addressed or resolved yet, and Andersen refers attendees to the Linux Kernel mailing list (LKML) for more information.

Next, Andersen talks about the Linux audit log. Regarding containers, the main thrust of the discussion is around namespacing audit, which shares many of the same challenges as namespacing IMA. Fortunately, there appears to be a workable solution to namespacing audit, and again Andersen refers attendees to LKML for more details.

The root of some of these problems with namespacing IMA and audit is, fundamentally, due to the fact that the kernel has no container construct. There is working going on, though, to introduce a container object in the kernel, which would standardize the representation of a container in the kernel. There are challenges and problems here; perhaps the most notable issue is that the current lack of a container object gives people flexibility in how they leverage containers and container technologies out of the kernel.

LSM (Linux Security Modules) are the next topic that Andersen addresses. There are a lot of LSMs: AppArmor, SELinux, etc. Namespacing LSMs is where the container-related work is happening; this would enable AppArmor on the host with SELinux in the guest/container (this isn't currently possible). You _can_ currently nest multiple AppArmor policies, but of course this is limited to AppArmor. Some additional work to enable nested SELinux inside of SELinux have been done, and there are some patches available to mix-and-match LSMs.

Next up is time namespacing, which would enable containers to have their own time namespace. At first, this seems natural, but the issue is the vDSO (virtual Dynamic Shared Object). vDSO is a way to speed up syscalls by mapping a shared page into userspace; time namespacing would break that. It's not impossible to fix, but quite difficult.

Moing away from namespacing, Andersen demonstrates seccomp logging (a feature that is actually in the kernel, he uses 4.14-rc for the demo). A fair amount of the demo was beyond my understanding, but hopefully it was helpful for others.

Coming back to LSMs again, Andersen talks about [Landlock][link-1], an eBPF LSM. eBPF is important because it allows people to programmatically extend the kernel.

Wireguard was on Andersen's list of topics, but he skips it because there was a (much too brief) demo of Wireguard in the previous session. Wireguard is positioned as a simpler alternative to IPSec and OpenVPN, but it lacks cypher agility (can't easily swap out cyphers in the event of a cypher weakness).

The last topic of Andersen's talk centers on the Kernel Self Protection Project (KSPP), a set of technologies for hardening the kernel. Specifically, Andersen talks about a project in which he is directly involved. The project is called XPFO (eXclusive Page Frame Ownership). The basic idea behind XPFO is that it protects against classic attacks overwriting kernel code with malicious functions, typically by calculating the correct memory address for a kernel memory page (which, according to a recent academic paper, can apparently be done about 96% of the time). XPFO protects against this attack, but is currently too cost-prohibitive (with regards to performance). Work is underway now to try to accelerate this feature so that it can be leveraged without heavily impacting performance.

Andersen then wraps up the session with a few additional resources, and then opens up for questions.



[link-1]: http://landlock.io/
