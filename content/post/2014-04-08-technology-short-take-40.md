---
author: slowe
categories: Information
comments: true
date: 2014-04-08T15:00:21Z
slug: technology-short-take-40
tags:
- Hardware
- HyperV
- iSCSI
- Networking
- OpenStack
- Security
- Storage
- Virtualization
- VMware
- VPLEX
- vSphere
- Windows
- Microsoft
- VPN
- OpenFlow
- SSL
title: 'Technology Short Take #40'
url: /2014/04/08/technology-short-take-40/
wordpress_id: 3428
---

Welcome to Technology Short Take #40. The content is a bit light this time around; I thought I'd give you, my readers, a little break. Hopefully there's still some useful and interesting stuff here. Enjoy!

## Networking

* Bob McCouch has a nice write-up on [options for VPNs to AWS](http://herdingpackets.net/2014/03/05/weighing-aws-vpn-options/). If you're needing to build out such a solution, you might want to read his post for some additional perspectives.

* Matthew Brender [touches](http://itechthereforeiam.com/2014/04/technical-short-the-complication-that-is-vmkernel-multi-homing/) on a networking issue present in VMware ESXi with regard to VMkernel multi-homing. This is something others have touched on before (including myself, back in [2008][1]---not 2006 as I tweeted one day), but Matt's write-up is concise and to the point. You'll definitely want to keep this consideration in mind for your designs. Another thing to consider: vSphere 5.5 introduces the idea of multiple TCP/IP stacks, each with its own routing table. As the ability to use multiple TCP/IP stacks extends throughout vSphere, it's entirely possible this limitation will go away entirely.

* YAOFC (Yet Another OpenFlow Controller), interesting only because it focuses on issues of scale (tens of thousands of switches with hundreds of thousands of endpoints). See [here](http://flowforwarding.github.io/loom/) for details.

## Servers/Hardware

* Intel recently announced a refresh of the E5 CPU line; Kevin Houston has more details [here](http://bladesmadesimple.com/2014/03/intel-e5-4600-v2-cpu-announced/).

## Security

* This one slipped past me in the last Technology Short Take, so I wanted to be sure to include it here. Mike Foley---whom I'm sure many of you know---recently published an ESXi security whitepaper. His [blog post](http://blogs.vmware.com/vsphere/2014/02/security-vmware-hypervisor-whitepaper.html) provides more details, as well as a link to download the whitepaper.

* The OpenSSL "Heartbleed" vulnerability has captured a great deal of attention (justifiably so). Here's [a quick article](http://www.howtoforge.com/find_out_if_server_is_affected_from_openssl_heartbleed_vulnerability_cve-2014-0160_and_how_to_fix) on how to assess if your Linux-based server is affected.

## Cloud Computing/Cloud Management

* I recently built a Windows Server 2008 R2 image for use in my OpenStack home lab. This isn't as straightforward as building a Linux image (no surprises there), but I did find a few good articles that helped along the way. If you find yourself needing to build a Windows image for OpenStack, check out [creating a Windows image on OpenStack](http://blog.gridcentric.com/bid/297627/Creating-a-Windows-Image-on-OpenStack) (via Gridcentric) and [building a Windows image for OpenStack](http://networkstatic.net/building-a-windows-image-for-openstack/) (via Brent Salisbury). You might also check out Cloudbase.it, which offers [a version of cloud-init for Windows](http://www.cloudbase.it/cloud-init-for-windows-instances/) as well as some prebuilt evaluation images. (Note: I was unable to get the prebuilt images to download, but YMMV.)

* Speaking of building OpenStack images, here's a "how to" guide on [building a Debian 7 cloud image for OpenStack](http://thornelabs.net/2014/04/07/create-a-kvm-based-debian-7-openstack-cloud-image.html).

* Sean Roberts recently launched a series of blog posts about various OpenStack projects that he feels are important. The [first project he highlights is Congress](http://sarob.com/2014/03/my-take-on-openstack-projects-congress-part-1-of-10/), a policy management project that has recently gotten a fair bit of attention (see a reference to Congress at the end of this recent article on [the mixed messages from Cisco on OpFlex](http://www.networkworld.com/community/blog/ciscos-mixed-messages)). In my opinion, Congress is a big deal, and I'm really looking forward to seeing how it evolves.

* I have a related item below under Virtualization, but I wanted to point this out here: work is being done on a VIF driver to connect Docker containers to Open vSwitch (and thus to OpenStack Neutron). Very cool. See [here](https://review.openstack.org/#/c/85913/) for details.

* I love that Cody Bunch thinks a lot like I do, like this quote from a recent post sharing some [links on OpenStack Heat](http://openstack.prov12n.com/openstack-heat-link-dump/): "That generally means I've got way too many browser tabs open at the moment and need to shut some down. Thus, here comes a huge list of OpenStack links and resources." Classic! Anyway, check out the list of Heat resources, you're bound to find something useful there.

## Operating Systems/Applications

* A short while back I had a Twitter conversation about spinning up a Minecraft server for my kids in my OpenStack home lab. That led to a few other discussions, one of which was how cool it would be if you could use Heat autoscaling to scale Minecraft. Then someone sends me [this](https://github.com/rackspace-orchestration-templates/minecraft).

* Per the Microsoft Windows Server Team's [blog post](http://blogs.technet.com/b/windowsserver/archive/2014/04/08/windows-server-2012-r2-update-is-now-generally-available.aspx), the Windows Server 2012 R2 Udpate is now generally available (there's also a corresponding update for Windows 8.1).

## Storage

* Did you see that EMC released a virtual edition of VPLEX? It's being called the "data plane" for software-defined storage. VPLEX is an interesting product, no doubt, and the introduction of a virtual edition is intriguing (but not entirely unexpected). I did find it unusual that the release of the virtual edition signalled the addition of a new feature called "MetroPoint", which allows two sites to replicate back to a single site. See Chad Sakac's [blog post](http://virtualgeek.typepad.com/virtual_geek/2014/04/vplex-virtual-edition-now-ga.html) for more details.

* This discussion [on MPIO and in-guest iSCSI](http://planetvm.net/blog/?p=2610) is a great reminder that designing solutions in a virtualized data center (or, dare I say it---a software-defined data center?) isn't the same as designing solutions in a non-virtualized environment.

## Virtualization

* Ben Armstrong [talks briefly about Hyper-V protected networks](http://blogs.msdn.com/b/virtual_pc_guy/archive/2014/03/11/protected-networks-in-windows-server-2012-r2.aspx), which is a way to protect a VM against network outage by migrating the VM to a different host if a link failure occurs. This is kind of handy, but requires Windows Server clustering in order to function (since live migration in Hyper-V requires Windows Server clustering). A question for readers: is Windows Server clustering still much the same as it was in years past? It was a great solution in years past, but now it seems outdated.

* At the same time, though, Microsoft is making some useful networking features easily accessible in Hyper-V. Two more of Ben's articles show off the [DHCP Guard](http://blogs.msdn.com/b/virtual_pc_guy/archive/2014/03/24/hyper-v-networking-dhcp-guard.aspx) and [Router Guard](http://blogs.msdn.com/b/virtual_pc_guy/archive/2014/03/25/hyper-v-networking-router-guard.aspx) features available in Hyper-V on Windows Server 2012.

* There have been a pretty fair number of posts talking about nested ESXi (ESXi running as a VM on another hypervisor), either on top of ESXi or on top of VMware Fusion/VMware Workstation. What I hadn't seen---until now---was how to get that working with OpenStack. [Here's how Mathias Ewald made it work](http://www.vxpertise.net/2014/03/nested-esxi-with-openstack/).

* And while we're talking nested hypervisors, be sure to check out William Lam's post on [running a nested Xen hypervisor with VMware Tools on ESXi](http://www.virtuallyghetto.com/2014/04/running-nested-xen-hypervisor-with-vmware-tools-on-esxi.html).

* Check out [this potential way](https://github.com/jbemmel/ecDock) to connect Docker containers with Open vSwitch (which then in turn opens up all kinds of other possibilities).

* Jason Boche regales us with a tale of a [vCenter 5.5 Update 1 upgrade that results in missing storage providers](http://www.boche.net/blog/index.php/2014/03/17/registered-storage-providers-missing-after-vcenter-5-5-update-1-upgrade/). Along the way, he also shares some useful information about Profile-Driven Storage in general.

* Eric Gray shares information on [how to prepare an ESXi ISO for PXE booting](http://www.vcritical.com/2014/03/automatically-prepare-an-esxi-iso-image-for-pxe-booting/).

* PowerCLI 5.5 R2 has some nice new features. Skip over to [Alan Renouf's blog](http://www.virtu-al.net/2014/03/13/powercli-5-5-r2-released/) to read up on what is included in this latest release.

I should close things out now, but I do have one final link to share. I really enjoyed Nick Marshall's recent post about [the power of a tweet](http://nickmarshall.com.au/blog/2014/3/8/the-power-of-a-tweet). In the post, Nick shares how three tweets---one with Duncan Epping, one with Cody Bunch, and one with me---have dramatically altered his life and his career. It's pretty cool, if you think about it.

Anyway, enough is enough. I hope that you found something useful here. I encourage readers to contribute to the discussion in the comments below. All courteous comments are welcome.

[1]: {{< relref "2008-12-19-vmware-esx-networking-articles.md" >}}
