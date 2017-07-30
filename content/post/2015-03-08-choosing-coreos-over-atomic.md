---
author: slowe
categories: Information
comments: true
date: 2015-03-08T16:00:00Z
tags:
- Linux
- Docker
- CLI
- CentOS
- CoreOS
title: Choosing CoreOS over Project Atomic
url: /2015/03/08/choosing-coreos-over-atomic/
---

Upon hearing [the news that Red Hat had released the Atomic Host variant of Red Hat Enterprise Linux][link-3], I decided that it would be a good idea for me to take a look at the [CentOS][link-5] flavor of the Atomic Host variant. In case you're unfamiliar, the Atomic Host variant is the result of [Project Atomic][link-2], which aimed to provide a container-optimized flavor of RHEL/CentOS/Fedora. This container-optimized flavor would leverage [rpm-ostree][link-1] for atomic system updates (hence the name) and come with [Docker][link-5] preinstalled. What I found, frankly, disappointed me.

Before I continue, I will make two very important disclaimers:

1. Note that there has been _no_ official announcement of the release of final builds of an Atomic Host variant for CentOS 7. So, it's entirely possible that all the issues I mention here are known issues that will be addressed. That being said, I did find CentOS 7 Atomic Host builds dated March 5, 2015; this is the same date as the Red Hat announcement. It's reasonable, therefore, to believe that these builds are very close to final builds.

2. It's entirely possible these issues are the result of errors on my part. I've spent most of my time with [Ubuntu][link-6] (for general purpose Linux use cases) and [CoreOS][link-7] (for container-optimized use cases). Therefore, it's definitely a possibility that my lack of familiarity with the "RHEL/CentOS" way of doing things led to some of the issues below.

With those two disclaimers out of the way, here are some general points that I noted about the CentOS 7 Atomic Host build I tested. (By the way, my testing was performed using [OpenStack][link-8] Icehouse running KVM on Ubuntu 12.04, with virtual networking provided by [VMware NSX][link-9].)

* At some point (it's unclear exactly when it happened or why it happened) cloud-init on the Atomic Host instances just stopped working. It wasn't the back-end cloud-init service (being provided by OpenStack), as that continued to work for other distributions/images. In total, I deployed between 10 and 15 Atomic Host instances. Customization via cloud-init worked for some number of them, but at some point it failed and I was completely unable to make cloud-init work for any instances after that point. Reviewing the console logs would show a traceback for cloud-init, meaning that cloud-init had reported an error and halted. All the while, cloud-init was working fine for instances based on Ubuntu and CoreOS images. I suppose it is possible that the CentOS Atomic Host image was corrupted or damaged in some way; I haven't yet tried to replace the image with a fresh download to see if that fixes anything.
* Once deployed, I found that the Docker daemon wouldn't start on Atomic Host instances. `systemctl -l status docker.service` would only report that the operation timed out, and `journalctl` didn't provide any additional detail. Rebooting the instance didn't help. Based on the results of some Internet searches, I thought perhaps the Docker daemon was having a problem setting up the default `docker0` bridge. Running `lsmod` showed that the iptables, conntrack, and nat kernel modules weren't loaded. Running Docker manually in non-daemon mode eventually (after waiting quite a while) caused those kernel modules to load (verified by running `lsmod` again), and at that point the Docker daemon would start via `systemctl`.
* The CentOS Atomic Host instances appeared to periodically "lock up"; for example, attempting to SSH into the instance would occasionally result in a _very_ long delay before getting logged in. I also experienced unexplained delays when running simple Docker commands (like `docker ps` to list containers once I had gotten Docker to work).
* Project Atomic does not provide any Atomic-specific cloud-init extensions that might simplify the customization of Atomic Host instances. This makes it much harder to do tasks like configuring the Docker daemon to listen on a network port, for example. That's not to say that it's impossible to perform these tasks, just that it is (in my opinion) much more difficult than it should be.
* Given that I couldn't automatically configure the Docker daemon to listen on a network port, I manually created the appropriate systemd socket unit. I verified that the Atomic Host instance was listening on port 2375, but all Docker commands sent to the daemon (via `docker -H tcp://<IP address>:2375`) simply hung. I verified there was no firewall configuration present that would cause the problem, but the issue persisted. I tried running Docker commands using `docker -H tcp://localhost:2375`, but those commands hung and become unresponsive as well.

All in all, the CentOS Atomic Host variant felt _unfinished_. It felt as if, perhaps in response to pressure from CoreOS, that Red Hat and contributors rushed to retrofit their existing distribution into a "container-optimized" variation. In contrast, CoreOS feels much more mature, more polished, and more production ready as a container-optimized Linux distribution. This is reflected in the cloud-init/cloud-config extensions to easily create and modify systemd units and in the _consistency_ of the distribution's behavior.

I'm sure that the RHEL/CentOS Atomic Host variants will get better over time, but for now I'm sticking with CoreOS.

### Update

Since publishing this post, I've had the opportunity to speak with someone from Red Hat to get some additional information, and I wanted to share that here:

* The officially-released version of RHEL Atomic Host is 7.1, _not_ 7.0. Thus, the 7.0 builds of CentOS Atomic Host with which I was testing actually pre-dated the official release (and, in fact, are no longer available for download). CentOS 7.1 Atomic Host builds should be coming soon. When the 7.1 builds are available, I'll test it again.
* Initial indications seem to point to an interaction between the use of rpm-ostree and cloud-init (version 0.7.5) as causing the cloud customization problems I described above. This isn't something the Red Hat team saw during their testing, but it's something into which they are currently looking.

My sincere thanks goes to Steve Gordon for taking the time to talk with me about the issues that I uncovered.


[link-1]: https://github.com/projectatomic/rpm-ostree
[link-2]: http://www.projectatomic.io
[link-3]: http://www.redhat.com/en/about/press-releases/red-hat-launches-red-hat-enterprise-linux-7-atomic-host-advances-linux-containers-enterprise
[link-4]: https://www.docker.com
[link-5]: https://www.centos.org
[link-6]: http://www.ubuntu.com
[link-7]: https://coreos.com
[link-8]: http://www.openstack.org
[link-9]: http://www.vmware.com/products/nsx/
