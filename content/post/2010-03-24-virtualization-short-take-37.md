---
author: slowe
categories: Information
comments: true
date: 2010-03-24T09:48:55Z
slug: virtualization-short-take-37
tags:
- Cisco
- HyperV
- Microsoft
- Nexus
- UCS
- Virtualization
- VMware
- vSphere
title: 'Virtualization Short Take #37'
url: /2010/03/24/virtualization-short-take-37/
wordpress_id: 1870
---

It's that time again: time for another Virtualization Short Take! My Virtualization Short Takes are quick glances at various news bytes, announcements, useful blog posts, or other items of interest. (By the way, the "short" in "Short Take" does not imply that my post is going to be short, in case anyone was wondering. I'm still long-winded, and I have a lot of things that I find interesting.)

* Have I mentioned how useful the [weekly VMware KB digests](http://blogs.vmware.com/kbdigest/) are?

* Frank Denneman has published a couple of really great articles recently. The first discussed [how to remove an orphaned Nexus 1000V distributed virtual switch](http://frankdenneman.nl/2010/03/removing-orphaned-nexus-dvs/); the second discusses a [complex interaction between HP Continuous Access and LUN balancing scripts](http://frankdenneman.nl/2009/02/hp-continuous-access-and-the-use-of-lun-balancing-scripts/). Both articles are worth a read.

* Similarly, Jeremy Waldrop has had a couple of good posts since he managed to get his hands on a Cisco UCS. The first post describes a "Doh!" moment when Jeremy realizes that [adding more vNICs to a VMware ESXi instance with the Cisco VIC](http://jeremywaldrop.wordpress.com/2010/03/13/presenting-4-vnics-to-vmware-esxi-4-with-the-cisco-ucs-vic-palo-adapter/) (aka "Palo"; sorry, Cisco, you're not going to be escaping that code name any time soon) is really just a matter of specifying them in the service profile. I can certainly see where that's not immediately intuitive. The other article describes Jeremy's [experience with using vNIC failover](http://jeremywaldrop.wordpress.com/2010/03/18/cisco-ucs-vnic-failover/). There's great information in the comments to that article; in particular, be sure not to enable vNIC failover with VMware vSphere. Bad things happen as a result. (OK, maybe not "bad things," but network connectivity can be adversely affected. You should let VMware vSphere handle the NIC teaming and failover.)

* Toni Westbrook has a good article on [how to move the COS VMDK](http://www.toniwestbrook.com/archives/168) in VMware ESX 4.0. Key note: this solution is currently unsupported by VMware, so use at your own risk.

* I've mentioned before how various bloggers often have a "masterpiece" post. This isn't necessarily their most well-written post, but it's the post that, for whatever reason, is a defining post for them. For me, it's the [ESX/VLANs/NIC teaming][1] article I wrote in 2006. I think Jason Boche might have just come up with his: an [in-depth discussion of the vpxd.cfg configuration options](http://www.boche.net/blog/index.php/2010/03/13/vpxd-cfg-advanced-configuration/). Great information, Jason!

* In VDI environments, storage capacity is only one aspect of the overall storage equation. Vijay Swami at vEverything takes a [pretty balanced view](http://virtualeverything.wordpress.com/2010/03/15/a-look-at-solving-the-vdi-iops-problem/) of how two leading storage vendors---EMC and NetApp---address not only storage capacity, but also IOPS. It's worth a read and again underscores that there is no "one right way" to do things. Different doesn't necessarily mean better or worse, just different. [It's all about the technology choices][2]. _(Disclosure: I work for EMC Corporation.)_

* [VDI on local disks, anyone?](http://myvirtualcloud.net/?p=448) It's an interesting discussion point that has its pros and cons. I guess the value of this sort of design really depends upon the business objectives the VDI implementation is trying to fulfill.

* Is anyone else amused by the abrupt "about face" that Microsoft performed with [Hyper-V's dynamic memory feature](http://blogs.technet.com/virtualization/archive/2010/03/18/dynamic-memory-coming-to-hyper-v.aspx)? Wow...even I was caught off-guard by how quickly they went from one end of the spectrum to the other. I would rather hear someone say, "You know, we were wrong, and this is a valuable feature after all" than to just flip 180 degrees and start moving in a whole new direction.

* Speaking of Microsoft and whole new directions...there was a great deal of coverage about Microsoft's desktop virtualization announcement. I won't try to delve into the details here; that's a particular niche that is better served by those who have the time to dedicate to it. If you haven't seen the news, my good friend Alessandro has a [great write-up](http://www.virtualization.info/2010/03/microsoft-announces-changes-in.html) and there's the [official press release](http://www.microsoft.com/Presspass/press/2010/mar10/03-18DesktopVirtPR.mspx) from Microsoft.

* If you're interested in getting more information on RemoteFX---which appears, more than anything else, to simply be a set of LAN-only acceleration features for RDP and not an entirely new protocol--[this article](http://blogs.technet.com/virtualization/archive/2010/03/17/explaining-microsoft-remotefx.aspx) has good information. You might also have a look at [this post about Service Pack 1](http://blogs.technet.com/windowsserver/archive/2010/03/18/announcing-windows-server-2008-r2-and-windows-7-service-pack-1.aspx) for Windows Server 2008 R2, which will enable both RemoteFX as well as the afore-mentioned Dynamic Memory.

* Continuing along with my little BSD love-fest, I came across this article that describes [some strange behavior with CARP](http://sysadminadventures.wordpress.com/2010/03/22/fixing-vm-based-pfsense-carp-announcement-echoes-when-using-teamed-network-adapters/) that can only be fixed by using link aggregation. The geek in me wants to go test this in a bunch of different scenarios to see if the Nexus 1000V fixes it or something like that, but I doubt that I'll have the time.

* This is old news now, but in case you hadn't heard [VMware is licensing technology from Likewise Software](http://www.likewise.com/blog/?p=216) for use with the next version of VMware vSphere. This will tighten vSphere's integration with Active Directory. This is generally good, except that it will render my articles on [ESX integration with Active Directory][3] useless.

* With VMware vSphere 4.0 Update 1, you can now install EMC PowerPath/VE using vCenter Update Manager. [This VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1018740) provides the details how it's done.

* If you're using ESXi and want to direct logging data elsewhere via syslog, [this VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1016621) describes to configure syslog in ESXi.

* The ages-old discussion of scale up vs. scale out is revisited again in [this blog post](http://itsjustanotherlayer.com/2010/03/scale-up-or-scale-out%e2%84%a2/). I guess the key takeaway for me is the reminder that while VMware HA does restart workloads automatically, there's still an outage. If you're running 50 VMs on a host, you're still going to have an outage across as many as 50 different applications within your organization. That's not a trivial event. I think a lot of people gloss over that detail. VMware HA helps, but it's not the ultimate solution to downtime that people sometimes portray it as.

* PHD Virtual has released esXpress version 4.0 today. I've taken a step back from most product announcements simply because they come too quickly to really keep up with them (unless you're a madman like David Marshall over at [VMBlog.com](http://vmblog.com/)---my hat's off to you, David!), but the timing worked out for this one. Go have a look at [PHD Virtual's web site](http://www.phdvirtual.com/) for all the details.

* Last, but most certainly not least, my esteemed colleague Mike Laverick has [completed his updated VMware SRM book](http://www.rtfm-ed.co.uk/2010/03/22/new-administrating-vmware-site-recovery-manager-4-0/), now updated for VMware SRM 4.0. Great work, Mike! I would wish you all financial success with the book, but as you're giving it away for free (an admirable step, by the way) I guess I'll just have to wish you all other forms of success!

That does it for me this time around, folks. Thanks for reading (I appreciate it!), and if you have some good information to add please feel free to speak up in the comments.

[1]: {{< relref "2006-12-04-esx-server-nic-teaming-and-vlan-trunking.md" >}}
[2]: {{< relref "2009-10-23-cutting-yourself-on-the-double-edged-sword.md" >}}
[3]: {{< relref "2007-07-10-esx-server-ad-integration.md" >}}
