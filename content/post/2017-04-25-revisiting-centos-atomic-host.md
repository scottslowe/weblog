---
author: slowe
categories: Information
comments: true
date: 2017-04-25T00:00:00Z
tags:
- Linux
- Docker
- CLI
- CentOS
- CoreOS
title: Revisiting CentOS Atomic Host
url: /2017/04/25/revisiting-centos-atomic-host/
---

A couple years ago, I wrote an article about how I was [choosing CoreOS over Project Atomic][xref-1] based on some initial testing with CentOS Atomic Host builds. As it turns out---and as I pointed out in the "Update" section of that article---the Atomic Host builds I was using were pre-release builds, and therefore it wasn't really appropriate to form an assessment based on pre-release builds. Now that both [CentOS Atomic Host][link-1] and [CoreOS][link-6] Container Linux have both grown and matured, I thought I'd revisit the topic and see how---if at all---things have changed.<!--more-->

In my original post, there were 4 major issues I identified (not necessarily in the same order as the original post):

* Lack of container-specific cloud-init extensions
* Difficulty customizing Docker daemon
* Odd issues with cloud-init
* Stability of the distribution

So how do these areas look now, 2 years later?

* _Container-specific cloud-init extensions:_ Upon a closer examination of this issue, I realized that the cloud-init extensions were actually specific to CoreOS projects, like etcd and fleet. Thus, it wouldn't make sense for these sorts of cloud-init extensions to exist on Atomic Hosts. What _would_ make sense would be extensions that help configure Atomic Host-specific functionality, though (to be honest) I can't think of any such functionality at the moment. I call this one a tie.

* _Difficulty customizing the Docker daemon:_ As evidenced in [this post][xref-2], I've clearly figured out how to customize the Docker daemon on an Atomic Host instance, using either shell scripts or Ansible playbooks. (I'm currently working on doing the same thing using `cloud-init` on AWS, but I still have some kinks to work out.) I'd say this one is a tie as well. (CoreOS may have a slight edge in having better documentation around this topic.)

* _Odd issues with cloud-init:_ I don't use [OpenStack][link-4] in my home lab any longer (no fault of OpenStack's, I simply don't run a home lab), so my `cloud-init` testing defaults to using user data when spinning up AWS instances based on a CentOS Atomic Host AMI. I haven't seen the issues that I saw in my testing 2 years ago, which were most likely due to the pre-release status of the version I tested at the time. Again, a tie.

* _Stability of the distribution:_ Because I'm not using OpenStack, the additional testing I've done here isn't quite "apples for apples" compared to the original article. However, I've been running more instances of CentOS Atomic Host, both via [VirtualBox][link-2] on my [Fedora Linux][link-3] laptop and on AWS, and haven't seen the same sort of instability I saw 2 years ago. As I mentioned in the update to the original article, my testing was done with pre-release builds, and that may explain some of the instability issues. Both distributions seem equally stable at this point.

Aside from these particular issues, there are still a number of differences between the two options:

* CoreOS Container Linux runs a _much_ newer Linux kernel, and supports newer container technologies like overlay/overlay2 file systems. CentOS Atomic Host runs Linux 3.10, and still relies on DeviceMapper for containers.
* CoreOS Container Linux publishes a JSON feed for their AWS AMIs, making it much easier [to find the latest CoreOS AMI ID][xref-3].
* CentOS Atomic Host includes Python, making it much easier to automate certain tasks using a tool like [Ansible][link-5].

So what's the final verdict? The answer certainly isn't as clear-cut as my misinformed view in the original post 2 years ago (see the updates on that original post for why I called my view misinformed). As with so many things in IT, it really boils down to requirements. Is there a need for some sort of host OS automation/configuration? If so, then Atomic Host might make more sense. Would the organization benefit from some consistency between a general-purpose OS (RHEL/CentOS) and a container-optimized OS (Atomic Host)? If not, then perhaps going with CoreOS Container Linux is the right approach. Either way, both are good options---for different reasons---for a container-optimized Linux distribution.



[link-1]: https://projectatomic.io/
[link-2]: https://www.virtualbox.org/
[link-3]: https://getfedora.org/
[link-4]: https://www.openstack.org/
[link-5]: https://www.ansible.com/
[link-6]: https://coreos.com/
[xref-1]: {{< relref "2015-03-08-choosing-coreos-over-atomic.md" >}}
[xref-2]: {{< relref "2017-03-02-customizing-docker-centos-atomic-host.md" >}}
[xref-3]: {{< relref "2017-03-29-easily-finding-latest-coreos-ami-id.md" >}}
