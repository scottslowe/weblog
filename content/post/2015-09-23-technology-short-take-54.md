---
author: slowe
categories: Information
comments: true
date: 2015-09-23T09:25:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- VMware
- NSX
- Arista
- Ansible
- Docker
- OpenStack
- Microsoft
- HyperV
- Windows
title: 'Technology Short Take #54'
url: /2015/09/23/technology-short-take-54/
---

Welcome to Technology Short Take #54! In this episode, I've gathered an odd collection of links and articles about key data center technologies. Without further ado, let's get to the content.

## Networking

- Not sure if this link belongs in Networking or in Virtualization, but we'll stick it here since it talks about VMware NSX. Here's a three-part series on running VMware NSX on vSphere on AWS via Ravello Systems ([part 1][link-6], [part 2][link-7], and [part 3][link-8]). This is a great way to get your feet wet with NSX without having to invest in a home lab.
- This is a bit of an older post, but I really appreciated Bob McCouch's post on [building tools versus "programming."][link-13] I think Bob really hit the nail on the head when he said that the real goal is working efficiently with high quality and low error rates. If this means you need to learn to write a script, then so be it. If it means it needs to be manual, then so be it (but please, _please_, do take the time to document it!).
- Dan Conde of ESG has [a write-up on the role of NSX in the new multi-platform world][link-23] in which VMware finds itself. It's a good summary of some of the new technology directions and features/functionality shown at the conference, so if you weren't able to attend this article can help bring you up to speed.
- Want to play with a spine/leaf fabric and LISP on Arista vEOS? No problem, [Ravello Systems has a blog (and a blueprint)][link-24] to help you set all this up.
- Darren O'Connor has an article describing how he [used bird to pull global BGP route counts][link-26]. ([Bird][link-27], by the way, is an open source routing daemon for Linux, *BSD, and other UNIX-like operating systems.)

## Servers/Hardware

Nothing this time around...but I'll watch out for items to include next time.

## Security

Sorry, Charlie! No security-focused links or articles to share this time around.

## Cloud Computing/Cloud Management

- Subbu Allamaraju has a great post, titled ["Lessons from the Cloud Bunker,"][link-1] in which he shares some lessons learned after a couple years of building large cloud infrastructure at eBay. The key takeaway, in my mind, is Subbu's statement that "Durable and declarative cloud native abstractions are the future of cloud." If you're charged with building cloud infrastructure and/or helping to leave a transformation to cloud-native applications, I'd highly recommend reading this post.
- From Stark & Wayne comes a two-part set of posts on deploying CloudFoundry on OpenStack using Terraform. (How's that for a mashup of technologies?) [The first post][link-3] is around gathering the various bits of information from OpenStack, while [the second post][link-4] talks about actually using Terraform to do the installation. (If you're doing this on OpenStack Juno, you'll also want to check [this post][link-5].)
- Want to run OpenStack with Hyper-V? Ben Armstrong has [a guide][link-9] on how to set it up.
- Joe Beda has a post that describes [the anatomy of a modern production stack][link-22]. Now, Joe freely admits that his experience is shaped and colored by his experience at Google, but the post is nevertheless useful (in my humble opinion) to help outline all the various components that are involved.

## Operating Systems/Applications

- Unikernels have been getting a bit more attention recently as the "future of computing." However, until now, most of the focus on unikernels has overlooked the operations side. Gareth Rushgrove tackles this topic head-on in his [discussion of operational challenges with unikernels][link-2]. I found the post very useful and informative, and helps put the idea of unikernels in place in the bigger overall picture.
- Alexander Brandstedt has a post on [using Docker Machine and AWS in conjunction with Ansible][link-11]. Basically, the post talks about using Docker Machine to provision an EC2 VM w/ Docker, and then leveraging information from Docker Machine (IP address, SSH key, etc.) to enable Ansible to run against that EC2 VM. Once Ansible is able to run, then you can configure it to allow Ansible to control Docker. It's a bit convoluted, but an interesting setup nevertheless.
- Billie Thompson has a write-up on [using Ansible to configure SystemD to control Docker containers][link-12].
- Creating YAML files to store data for use in Vagrant isn't a new idea, but what I hadn't seen before was creating multi-NIC setups in YAML for use with Vagrant. Here's [a write-up][link-14] by Larry Smith Jr. on how to make it work.
- Sometimes, the best way to learn something new is to connect and associate it with something you already know. That's why I'm including these links on using Docker and Google Compute Engine to run a Minecraft server ([part 1 on using GCE and Docker][link-16], [part 2 on persistent storage with a containerized Minecraft server][link-17]). Lots of folks out there like to play Minecraft, and perhaps even run their own Minecraft servers---so by connecting something you already know with something new, I can make it easier to learn. You're welcome.
- Thomas Maurer has a nice post on [first steps with Windows Containers][link-25]. One of the things I liked about this post is that he uses some nice graphics (not sure if they are his or from Microsoft) that really help illustrate some of the differences between Windows Containers, Hyper-V Containers, and Hyper-V VMs.
- Want more Docker links? [Here you go][link-29].

## Storage

- VSAN 6.1 is all the rage right now, especially the new Stretched Cluster functionality. William Lam shows [how to deploy and run the VSAN 6.1 Witness Virtual appliance on VMware Fusion or VMware Workstation][link-20] so that you can set up your own 2-node configuration (for testing or evaluation purposes; per William, this configuration isn't officially supported).
- Although I'm not familiar with DynamoDB, I found [this article on using DynamoDB in production][link-21] to be very interesting (and informative) reading. This article, for me, _really_ underscores the difficulty in building distributed systems. "Moving it to the cloud" may solve some problems, but it's bound to introduce new ones, too---so plan ahead.

## Virtualization

- Luc Dekens has recently released [a PowerShell module for Ravello Systems][link-18]. Ravello, as you probably already know, has offers a solution to allow users to run VMware VMs (and even VMware vSphere itself) on public clouds like AWS and GCE. Now Luc brings the ability for users to automate this functionality via PowerShell. Nifty.
- Akira Ohio has put together quite [an impressive guide][link-28] to using VMware AppCatalyst Technical Preview 2 (TP2) to learn about Docker and related tools like Docker Machine and Docker Compose. It's definitely worth having a look if you are a Mac user and in need of mastering some Docker basics.
- Iwan Rahabok---a colleague at VMware and a friend---has a useful post on [determining whether your ESXi uplinks are saturated][link-30] (he's using vRealize Operations).
- PowerCLI 6.0 Release 2 is now generally available; see [the announcement][link-31] on the PowerCLI blog for all the details. (Also see [Alan Renouf's blog post][link-33] about this release.)
- Will van Antwerpen [points out][link-32] that Unity support for Linux has been removed from VMware Workstation 12 (per the release notes). That's a bummer.
- Hyper-V on Nano in Windows Server 2016 TP3? Or how about Nano from Windows Server 2016 TP3 as a Hyper-V guest? Ben Armstrong has both---see [here][link-34] and [here][link-35].

## Career/Soft Skills/Productivity

- A lot of emphasis is placed on Inbox Zero, but in placing so much emphasis on an empty Inbox I think we've _lost the reason_ why we strive for an empty Inbox (hint: it's to get things done). So 99U has [an article on an alternate approach][link-10] that might be a good reminder that getting work done is the goal, not just emptying your Inbox.
- Along the same lines---and possibly related to the same post at 99U---is a write-up on [how time management is only making our lives worse][link-15]. I especially like the phrase "time confetti," and appreciate the emphasis that it's really our attention (our focus) that needs to be more carefully managed, not necessarily our time. Good stuff.
- Todd Scalzott shares [a brief post on all the excuses not to write][link-19], but wraps it up with this statement: "Because I'd like to read what you have to say." Well said, Todd.

Well, that's all this time around. I hope you found something useful. Feel free to hit me up on Twitter if you have resources or links that you think I should include in the next Technology Short Take. Thanks!



[link-1]: https://www.subbu.org/blog/2015/08/lessons-from-the-cloud-bunker
[link-2]: http://www.morethanseven.net/2015/08/21/operating-unikernel-challenges/
[link-3]: https://blog.starkandwayne.com/2015/05/06/configuring-openstack-for-cloud-foundry/
[link-4]: https://blog.starkandwayne.com/2015/05/06/deploying-cloud-foundry-on-openstack-using-terraform/
[link-5]: https://blog.starkandwayne.com/2015/05/05/openstack-juno-static-ip-patch/
[link-6]: http://nsx.world/nsx-on-aws-part-1/
[link-7]: http://nsx.world/nsx-on-aws-part-2/
[link-8]: http://nsx.world/nsx-on-aws-part-3/
[link-9]: http://blogs.msdn.com/b/virtual_pc_guy/archive/2015/08/25/getting-hyper-v-and-openstack-setup-quickly.aspx
[link-10]: http://99u.com/workbook/51514/screw-inbox-zero-heres-a-better-plan/
[link-11]: http://qone.io/docker/docker-machine/ansible/aws/2015/06/13/docker-ansible-aws-docker-machine.html
[link-12]: https://purplebooth.co.uk/blog/2015/05/31/managing-docker/
[link-13]: http://herdingpackets.net/2015/02/27/thoughts-on-building-tools-versus-programming/
[link-14]: http://everythingshouldbevirtual.com/vagrant-multi-nic-definitions-via-yaml
[link-15]: http://qz.com/447193/time-management-is-only-making-our-busy-lives-worse/
[link-16]: http://www.blog.juliaferraioli.com/2015/06/running-minecraft-server-on-google.html
[link-17]: http://www.blog.juliaferraioli.com/2015/07/saving-world-using-persistent-storage.html
[link-18]: http://www.lucd.info/2015/08/28/ravello-powershell-module/
[link-19]: http://notscott.blogspot.com/2015/09/musings-of-infrequent-blogger_17.html
[link-20]: http://www.virtuallyghetto.com/2015/09/how-to-deploy-and-run-the-vsan-6-1-witness-virtual-appliance-on-vmware-fusion-workstation.html
[link-21]: http://blog.sendwithus.com/using-dynamodb-production/
[link-22]: http://www.eightypercent.net/post/layers-in-the-stack.html
[link-23]: http://www.esg-global.com/blogs/vmworld-2015-a-multi-platform-world-the-role-of-nsx/
[link-24]: https://www.ravellosystems.com/blog/lisp-leaf-spine-architecture-arista-veos-on-aws/
[link-25]: http://www.thomasmaurer.ch/2015/09/first-steps-with-windows-containers/
[link-26]: https://mellowd.co.uk/ccie/?p=5771
[link-27]: http://bird.network.cz
[link-28]: https://www.penflip.com/akira.ohio/appcatalyst-hands-on-lab-en
[link-29]: http://www.nkode.io/2014/08/24/valuable-docker-links.html
[link-30]: http://virtual-red-dot.info/is-any-of-your-esxi-vmnics-saturated/
[link-31]: https://blogs.vmware.com/PowerCLI/2015/09/powercli-6-0-release-2-is-now-generally-available.html
[link-32]: http://planetvm.net/blog/?p=2936
[link-33]: http://www.virtu-al.net/2015/09/17/powercli-6-0-r2-the-most-advanced-version-vmware-has-ever-made/
[link-34]: http://blogs.msdn.com/b/virtual_pc_guy/archive/2015/09/08/running-hyper-v-on-nano-in-windows-server-2016-tp3.aspx
[link-35]: http://blogs.msdn.com/b/virtual_pc_guy/archive/2015/09/14/running-nano-from-windows-server-2016-tp3-on-hyper-v.aspx