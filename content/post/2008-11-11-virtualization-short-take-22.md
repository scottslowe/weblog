---
author: slowe
categories: Information
comments: true
date: 2008-11-11T11:08:14Z
slug: virtualization-short-take-22
tags:
- Citrix
- Fusion
- HyperV
- NetApp
- NFS
- Virtualization
- VMFS
- VMware
- VMwareHA
title: 'Virtualization Short Take #22'
url: /2008/11/11/virtualization-short-take-22/
wordpress_id: 1024
---

Despite the fact that I'm out of town this week at NetApp Insight, I wanted to go ahead and get out the latest installation of Virtualization Short Takes---my sometimes-weekly collection of interesting or useful links and tidbits.

* Much ado has been made about [VMware's acquisition of Trango](http://www.virtualization.info/2008/11/vmware-acquires-trango-hypervisor-is.html) and [the announcement of VMware MVP](http://vmware.com/company/news/releases/mvp.html) (Mobile Virtualization Platform). Rich Brambley has [a great write-up](http://vmetc.com/2008/11/11/what-does-vmware-mvp-provide-for-vdi-in-the-cloud-businesses-and-users/), and I completely agree with Rich and Alex Barrett about what this _really_ means: don't expect to see Windows XP on your smartphone any time soon. Alex [said it best](http://servervirtualization.blogs.techtarget.com/2008/11/10/vmware-mvp-does-not-equal-windows-xp-on-your-phone/): this is virtualization, not emulation, and Windows XP doesn't run on ARM.

* I'm curious---how many people agree with my comments in Alex's article about [the Citrix ICA client for the iPhone](http://searchservervirtualization.techtarget.com/news/article/0,289142,sid94_gci1337825,00.html). Is there any real, actual value in being able to access a Windows session from your iPhone? I tend to think not, but it would be an interesting discussion. Speak up in the comments.

* Duncan points out that the [issue with adding NICs to a team](http://www.yellow-bricks.com/2008/11/07/scripted-installs-and-nic-teaming/) and keeping them all active---the workaround for which required editing `esx.conf`---has now been fixed in ESX 3.5 Update 3. It's now possible to add NICs using `esxcfg-vswitch` and there's no need to edit `esx.conf`. Excellent!

* If you haven't yet checked out [Leo's Ramblings](http://lraikhman.blogsite.org/), go give it a look. He's got some good content. It's worth subscribing to the RSS feed (I did).

* Rick provides a [helpful tool](http://www.vmwarewolf.com/common-system-management-issues-in-vmware-infrastructure/) to resolving common system management issues with VMware Infrastructure. Thanks, Rick!

* Regular readers may recall that Chad Sakac of EMC and I had a round of VMware HA-related posts a few months ago (check out the [VMwareHA](/tags/vmwareha/) tag for a full list of VMware HA-related posts). As part of that discussion there was lots of information provided about Service Console redundancy, failover NICs, secondary Service Console connections, additional isolation addresses...all sorts of good stuff. Duncan joined in the conversation as well with a number of great posts, and has been keeping it up since then. His latest contribution to the conversation is a comparison of using [failover NICs vs. using a secondary Service Console](http://www.yellow-bricks.com/2008/10/22/dasfailuredetectiontime-for-activestandby-cos-vswitch/) to prevent VMware HA isolation response. According to the documentation, using a secondary Service Console can help reduce the wait time for VMware HA to step in should isolation actually occur. Good stuff, and definitely worth some additional exploration in the lab.

* As a sort of follow-up to the discussion about using NFS for VMware, [this VMware Communities thread](http://communities.vmware.com/message/998849;jsessionid=8A39D2587146F5DEBDE5872BA14203AB) has some great information on _why_ the NFS timeouts should be increased in NetApp NFS environments. If you're like me, you like to know the reasons behind the recommendations, and this thread was very helpful to me. Let me also add that we've recently started recommended to customers to increase their Service Console memory to 800MB when using NFS, so that might be something to consider as well.

* Need to change the path of where Update Manager stores its patches? Gabe shows you how [here](http://www.gabesvirtualworld.com/?p=28).

* Eric Gray of VCritical explores the question: [what would things be like without VMFS?](http://www.vcritical.com/2008/10/what-would-things-be-like-without-vmfs/) Well, as he states, you can just ask a Hyper-V user, since Hyper-V doesn't (yet) have a shared cluster filesystem. Yes, that will change in 2010 with Shared Cluster Volumes in Windows Server 2008 R2 and Hyper-V 2.0. I know. Or you can just add Melio FS from Sanbolic today and get the same thing. This is not anything new to me; I discussed this quite extensively [here][1] and [here][2]. Now, what would _really_ be interesting is for VMware to work with Sanbolic to co-develop a more advanced version of VMFS that eliminates the SCSI reservation problems...

* Need a nice summary of the various network communications that occur between different components of a VI3 implementation? Look no further than [right here](http://www.boche.net/blog/?p=323). Jason's site is another one that's worth adding to your RSS reader.

* If you really like living on the edge, here's a collection of [some RPMs for VMware ESX 3.5](http://blog.barfoo.org/projects/rpms-for-vmware-esx-35). Keep in mind that installing third-party RPMs like this is not recommended or supported...

* Andy Leonard [picked up](http://andyleonard.com/2008/11/06/why-im-kinda-looking-forward-to-vi-4/) the DPM video by VMware and is looking forward to DPM no longer being experimental. What he'd really like, though, is some feature to move his VMs via Storage VMotion and spin down idle disks. Andy, I wouldn't hold my breath.

* If you are a Fusion user (if you own a Mac and need to run Windows, you _should_ be a Fusion user!), [this hint](http://www.macosxhints.com/article.php?story=20081024151237960) might come in handy.

* Eric Siebert has a good post on [traffic flow between VMs](http://itknowledgeexchange.techtarget.com/virtualization-pro/how-traffic-routes-between-vms-on-esx-hosts/) in various configuration scenarios---different vSwitches, same vSwitches, different port groups, same port groups, etc. Have a look if you are at all unclear how VMware ESX handles traffic flow.

That does it for this round. Speak up in the comments if you have any interesting or useful links to share with other readers. I'd also be interested in readers' thoughts on the whole Citrix on the iPhone discussion---will it really bring any usefulness?

[1]: {{< relref "2008-06-10-a-discussion-with-jeff-woolsey.md" >}}
[2]: {{< relref "2008-06-16-melio-fs-hyper-v-and-vmware-esx.md" >}}
