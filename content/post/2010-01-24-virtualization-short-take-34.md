---
author: slowe
categories: Information
comments: true
date: 2010-01-24T19:02:58Z
slug: virtualization-short-take-34
tags:
- EMC
- Fusion
- HP
- Nexus
- Virtualization
- VMware
title: 'Virtualization Short Take #34'
url: /2010/01/24/virtualization-short-take-34/
wordpress_id: 1807
---

Welcome to Virtualization Short Take #34, my occasionally-weekly collection of virtualization-related links, posts, and comments. As usual, this information is a hodge-podge of information I've gathered from across the Internet over the last few weeks. I hope that you find something useful or helpful here, and thanks for reading!

* First up is Arne Fokkema's [PowerCLI script to check Windows VM partition alignment](http://ict-freak.nl/2009/12/15/powercli-check-partition-alignment-windows-vms-only/). As one commenter pointed out, the fact that the starting offset isn't 65536---which is what Arne's script checks---doesn't necessarily mean that it isn't aligned. Generally, you can align a Windows partition by setting the starting offset to any number that is evenly divisible by 4096 (4K). If I'm not mistaken, setting the partition offset to 65536 (64K) also ensures that the partition is stripe-aligned on EMC arrays.

* Here's [a useful reminder](http://blog.webactivedirectory.com/2010/01/16/dont-run-dns-as-a-virtual-machine/) to be sure to keep your dependencies in mind when designing VMware vSphere 4 environments. If you design your environment to rely upon DNS---a common situation, since VMware HA is particularly sensitive to name resolution---then be sure to appropriately architect the DNS infrastructure. This "circular dependency" is one reason why I personally tend to keep vCenter Server on a physical system. Otherwise, you have the virtualization management solution running on the infrastructure it is responsible for managing. (Yes, I know that it's fully supported for it to be virtualized and such.)

* Forbes Guthrie's article on [incorporating Active Directory authentication and sudo into the kickstart process](http://www.vreference.com/2010/01/14/ad-and-sudo-integratation-in-kickstart/) is a good read. With regard to his note about enabling root SSH access because of an inability to access the Active Directory DCs: I know that in ESX 3.x you could [still log in at the Emergency Console][1] when Active Directory connectivity was unavailable; does anyone know if this is still the case with ESX 4.0? I haven't taken the time to test it yet.

* Oh, and speaking of Active Directory authentication, Forbes also published [this note](http://www.vreference.com/2010/01/20/esx-4-1-to-include-likewise-ad-authentication/) about Likewise AD authentication supposedly included in ESX 4.1. Looks like someone at Likewise accidentally spilled the beans...

* I'm sure that everyone has seen the article by Duncan about the ESX 3.x bug that [prevents NIC teaming load balancing from working on the global vSwitch configuration](http://www.yellow-bricks.com/2010/01/20/nic-teaming-load-balancing-does-not-work-with-global-vswitch-configuration-on-esx-3-5/), but if you haven't---well, now you have. Here's the [corresponding KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1013691), also linked from Duncan's article. Duncan also recently published a note about an error while installing vCenter Server that is related to permissions; read it [here](http://www.yellow-bricks.com/2010/01/22/setup-cannot-create-vcenter-server-directory-services-instance-error-28038/).

* Are there even better days ahead for virtualization and those involved in virtualization? David Greenfield of Network Computing [seems to think so](http://www.networkcomputing.com/virtualization/virtualization-even-better-days-ahead.php). The comments in the article do seem to bear out my statements that virtualization experts now need to move beyond consolidation and start helping customers tackle the Tier 1, high-end applications. I believe that this is going to require more planning, more expertise, and more knowledge of the applications' behaviors in order to be successful.

* Stephen Dion of virtuBLOG brings up [a compatibility issue](http://virtublog.com/2010/01/24/esx-4-0u1-intel-quad-nic-issue/) with Intel quad-port Gigabit Ethernet network adapters when used with VMware ESX 4.0 Update 1. Anyone have any updates or additional information on this issue?

* If you're considering virtualizing Exchange Server 2010 on VMware vSphere, be sure to read Kenneth's article here about [Exchange 2010 DAGs and VMotion](http://kennethvanditmarsch.wordpress.com/2009/11/20/vmotion-and-exchange-2010/). At least live migration isn't supported on Hyper-V, either.

* Want to run a VM inside a VM? This post on [nested VMs](http://communities.vmware.com/docs/DOC-8970) over at the VMware Communities site has some very useful information.

* Paul Fazzone (who I believe is a product manager for the Nexus 1000V) [points out](http://faz1.com/blog/2010/01/24/nexus-1000v-vs-the-default-vmware-vswitch/) a good [point-counterpoint article](http://searchnetworking.techtarget.com.au/articles/38282-VMware-vSwitch-vs-Cisco-Nexus-1-V) with Bob Plankers and David Davis that discusses the benefits and drawbacks of the Cisco Nexus 1000V. Both writers make excellent points; I guess the real conclusion is that both options offer value for different audiences. Some organizations will prefer the VMware vSwitch (or Distributed vSwitch); others will find value in the Cisco Nexus 1000V. Choice is a beautiful thing.

* Jason Boche published [some performance numbers](http://www.boche.net/blog/index.php/2010/01/23/vmtn-storage-performance-thread-and-the-emc-celerra-ns-120/) for the EMC Celerra NS-120 that he's recently added to his home "lab" (I use the term "lab" rather loosely here, considering the amount of equipment found there). Not surprisingly, Fibre Channel won out over software iSCSI and NFS, but Jason's numbers showed a larger gap than many expected. I may have to repeat these tests myself in the EMC lab in RTP to see what sorts of results I see. If only I still had the NS-960 that I used to have at ePlus....sigh.

* Joep Piscaer has a [good post on Raw Device Mappings (RDMs)](http://www.virtuallifestyle.nl/2010/01/recommended-detailed-material-on-rdms/) that definitely worth a read. He's pulled together a good summary of information on RDMs, such as requirements, limitations, use cases, and frequently asked questions. Good job Joep!

* Ivo Beerens has a pretty detailed post on [multipathing best practices for VMware vSphere 4 with HP EVA storage](http://www.ivobeerens.nl/?p=465). The recommendation is to use Round Robin with ALUA and to reduce the IOPS limit to 1. Ivo also presents a possible workaround to the IOPS "random value" bug that Chad Sakac discussed [in this post](http://virtualgeek.typepad.com/virtual_geek/2009/12/vsphere-4-nmp-rr-iooperationslimit-bug-and-workaround.html) some time ago.

* Here's yet another great diagram by Hany Michael, this time on [ESX memory management and monitoring](http://www.hypervizor.com/2010/01/diagram-esx-memory-management-and-monitoring-v1-0/).

* This post tells you [how to modify your VMware Fusion configuration files](http://www.nileshk.com/vmware-fusion-nat-dhcp-and-port-forwarding) to assign IP addresses for NAT-configured VMs. If you're familiar with editing `dhcpd.conf` on a Linux system, the information found here on customizing Fusion should look quite familiar.

* Back in 2007, I wrote a piece on [using link state tracking in blade deployments][2]. This post wasn't necessarily virtualization focused, but certainly quite applicable to virtualization environments. Recently I saw [this article](http://shaw38.wordpress.com/2010/01/22/link-state-tracking-vmware-esx-and-you/) pop up on using link state tracking with VMware ESX environments. It's good to see more people recommending this functionality, which I feel is quite useful.

* Congratulations to Mike Laverick of RTFM, who this past week announced that [TechTarget is acquiring RTFM](http://www.rtfm-ed.co.uk/?p=2062) and its author, much like TechTarget acquired BrianMadden.com (and its author) last year. Is this a new trend for technical blog authors---build up a readership and then "sell it off" to a digital media company?

Here are some additional links that I stumbled across, but for which I haven't yet fully assimilated or processed. You might see some more in-depth blog posts about these in the near future as they work their way through my consciousness.

[Lab Experiment: Hypervisors](http://virtualizationreview.com/articles/2009/03/02/lab-experiment-hypervisors.aspx) (Virtualization Review)  

[The Backup Blog: Avamar and VMware Backup Revisited](http://thebackupblog.typepad.com/thebackupblog/2010/01/avamar-and-vmware-backup-revisited.html)  

[VMware vSphere Capacity IQ Overview - I'm Impressed!](http://www.virtualinsanity.com/index.php/2010/01/11/vmware-vsphere-capacity-iq-overview-im-impressed/)

Well, that wraps it up for now. Thanks for reading and feel free to speak out in the comments below.

[1]: {{< relref "2008-01-09-local-logins-refused-in-ad-integration-scenarios.md" >}}
[2]: {{< relref "2007-06-22-link-state-tracking-in-blade-deployments.md" >}}
