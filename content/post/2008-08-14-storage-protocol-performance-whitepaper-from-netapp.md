---
author: slowe
categories: Rant
comments: true
date: 2008-08-14T09:41:03Z
slug: storage-protocol-performance-whitepaper-from-netapp
tags:
- FibreChannel
- iSCSI
- NAS
- NetApp
- NFS
- SAN
- Storage
- Virtualization
- VMware
title: Storage Protocol Performance Whitepaper from NetApp
url: /2008/08/14/storage-protocol-performance-whitepaper-from-netapp/
wordpress_id: 821
---

NetApp recently published a white paper summarizing some tests they ran to compare [storage protocol performance in a VMware Infrastructure environment](http://media.netapp.com/documents/tr-3697.pdf). The white paper, TR-3697, compares the storage performance of Fibre Channel, software iSCSI, and NFS against a couple of different NetApp storage systems.

I won't go into all the sordid details here---you can read the white paper yourself---but the end results look something like this:

* Fibre Channel provided the highest throughput and the lowest processor utilization of all the storage protocols.

* Software iSCSI provided only slightly lower throughput than Fibre Channel (not more than 9% or 10% less than Fibre Channel depending upon the specific tests being run). However, software iSCSI consistently showed the highest CPU utilization on the ESX hosts.

* NFS showed throughput on the same levels as software iSCSI (again, not more than about 9% or 10% less than Fibre Channel depending upon the tests being run) and had higher CPU utilization than Fibre Channel. However, the CPU utilization was lower than with software iSCSI.

While overall performance was _roughly_ comparable between all three storage protocols, depending upon the tests being run, the host CPU utilization was a different story entirely. In some cases, software iSCSI's CPU utilization was as much as 80%--that's right, almost double---that of Fibre Channel. In no cases did the CPU utilization drop below 40% higher than Fibre Channel. Keep in mind these numbers are _relative_ to Fibre Channel. So if Fibre Channel used 200MHz of host CPU power and software iSCSI used 360MHz of host CPU power, that's an 80% relative increase. We don't know, unfortunately, how this translates into actual host CPU usage; in my mind, that's a key piece of information that really should have been included. I'm puzzled as to why it's not included.

NFS fared better; at its worst, the tests showed NFS running CPU overhead 40% greater than Fibre Channel. At its best, NFS looked like it was only requiring about 15% more CPU overhead than Fibre Channel (keep in mind the comments made above regarding relative utilization). Of course, NetApp loves to push the NFS; the document adds the extra sell for NFS:

>While NFS does not quite achieve the performance of FC and has a slightly higher CPU utilization, it does have some advantages over FC that should be considered when deciding which protocol to deploy. Running on a standard TCP/IP network, NFS does not require the expensive Fibre Channel switches, host bus adapters, and Fibre Channel cabling that FC requires, making NFS a lower cost alternative of the two protocols. Additionally, operational costs are low with no specialized staffing or training needed in order to maintain the environment. Also, NFS provides further storage efficiencies by allowing on-demand resizing of data stores and increasing storage saving efficiencies gained when using deduplication. Both of these advantages provide additional operational savings as a result of this storage simplification.

I suppose I can't blame them; NFS is one of their strong points, so they'll naturally lean that direction.

There are a few key things that I need to say about this document, though:

1. Benchmark tests can be made to say just about anything. It's all in the types of tests that you run and the parameters of those tests. I'm not saying that NetApp specifically skewed the tests in any way; what I am saying, though, is that users need to take these types of benchmark tests as a general guideline and not the definitive word.

2. While NetApp does highlight the "operational savings" of NFS, what they fail to mention is the added complexity of scaling NFS traffic as the environment grows. Fibre Channel multipathing in a VMware environment is very robust, and I expect that the Round Robin pathing policy will move from "experimentally supported" to fully supported rather quickly. This makes it quite easy to scale the FC connection, although to be honest that probably won't be necessary. However, to scale the NFS connection, you need multiple NFS exports with multiple IP addresses, link aggregation via LACP/802.3ad/EtherChannel and switches that support cross-switch link aggregation, and possibly multiple VMkernel ports on different IP subnets. This is described, by the way, in the [latest revision of TR-3428](http://www.netapp.com/us/library/technical-reports/tr-3428.html), also from NetApp. (As a side note, I believe that these scaling issues would affect _any_ NFS storage vendor and are not specific to NetApp in any way.)

3. If you look at VMware's development, you will see that Fibre Channel gets the goods the earliest. iSCSI and NFS were only added in VMware Infrastructure 3, whereas Fibre Channel support has been around in ESX for much longer. Storage VMotion support went to Fibre Channel first. VCB support went to Fibre Channel first. SRM support went to both iSCSI and Fibre Channel, but not NFS. Fibre Channel multipathing is, as I mentioned already, quite robust; iSCSI multipathing and NFS multipathing aren't quite so robust. All these things considered, there could be a sound business case to use Fibre Channel in spite of cost savings from iSCSI (especially software iSCSI, given the added CPU overhead) or NFS. That's something that each individual organization will need to decide for themselves.

By the way, I know the gentleman that wrote this technical report and he's a straight-up guy. I respect him. So, don't take any of my comments or thoughts to imply anything beyond the fact that I'm simply presenting my thoughts around the data contained in this document. You should also know that I _am_ a fan of using NFS for VMware, but I don't necessarily believe that it is the "slam dunk" that it's often presented to be.

**UPDATE:** I've made some corrections to the interpretations of the CPU utilization numbers in response to some of the comments below.
