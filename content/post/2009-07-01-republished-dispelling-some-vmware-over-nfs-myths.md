---
author: slowe
categories: Rant
comments: true
date: 2009-07-01T09:13:08Z
slug: republished-dispelling-some-vmware-over-nfs-myths
tags:
- NFS
- Storage
- Virtualization
- VMware
title: 'Republished: Dispelling Some VMware over NFS Myths'
url: /2009/07/01/republished-dispelling-some-vmware-over-nfs-myths/
wordpress_id: 1434
---

_Author's Note: This content was first published over at [Storage Monkeys](http://www.storagemonkeys.com), but it appears that it has since disappeared and is no longer available. For that reason, I'm republishing it here (with minor edits). Where applicable, I'll also be republishing other old content from that site in the coming weeks. Thanks!_

In this article, I'm going to tackle what will probably be a sensitive topic for some readers: VMware over NFS. All across the Internet, I run into article after article after article that sings the praises of NFS for VMware. Consider some of the following examples:

* [VMware over NFS - Why?](http://www.vifaq.com/index.php?option=com_content&task=view&id=25&Itemid=2)

* [Why VMware over NetApp NFS](http://viroptics.blogspot.com/2007/11/why-vmware-over-netapp-nfs.html)

* [VMware & NFS](http://www.vm-aware.com/2008/02/28/vmware-nfs/)

That first link looks to be mostly a reprint of [this blog post by Nick Triantos](http://storagefoo.blogspot.com/2007/09/vmware-over-nfs.html). Now, Nick is a solid storage engineer; there is no question in my mind that he knows Fibre Channel, iSCSI, and NFS inside out. Nick is certainly someone who is more than qualified to speak to the validity of using NFS for VMware storage. _But..._

I am going to have to disagree with some of the statements that are being propagated about NFS for VMware storage. Is NFS for VMware environments a valid choice? Yes, absolutely. However, there are some myths about NFS for VMware storage that need to be addressed.

1. _**Myth #1: All VMDKs are thin provisioned by default with NFS, and that saves significant amounts of storage space.**_ That's true---to a certain point. What I pointed out back in March of 2008, though, was that these VMDKs are [only thin provisioned at the beginning][1]. What does that mean? Perform a Storage VMotion operation to move those VMDKs from one NFS datastore to a different NFS datastore, and the VMDK will inflate to become a thick provisioned file. Clone another VM from the VM with the thin provisioned disks, and you'll find that the cloned VM has thick VMDKs. That's right---the only way to get those thin provisioned VMDKs is to create all your VMs from scratch. Is that what you really want to do? _(Note: VMware vSphere now supports thin provisioned VMDKs on all storage platforms, and corrects the issues with thin provisioned VMDKs inflating due to a Storage VMotion or cloning operation, so this point is somewhat dated.)_

2. _**Myth #2: NFS uses Ethernet as the transport, so I can just add more network connections to scale the bandwidth.**_ Well, not exactly. Yes, it is possible to add Ethernet links and get more bandwidth. However, you'll have to deal with a whole list of issues: link aggregation/802.3ad, physical switch redundancy (which is further complicated when you want to use link aggregation/802.3ad), multiple IP addresses on the NFS server(s), multiple VMkernel ports on the VMware ESX servers, and multiple IP subnets. Let's just say that scaling NFS bandwidth with VMware ESX isn't as straightforward as it may seem. This article I wrote back in July of 2008 may help shed some light on the particulars that are involved when it comes to [ESX and NIC utilization][2].

3. _**Myth #3: Performance over NFS is better than Fibre Channel or iSCSI.**_ Based on [this technical report by NetApp](http://media.netapp.com/documents/tr-3697.pdf)--no doubt one of the biggest proponents of NFS for VMware storage---NFS performance trails Fibre Channel, although by less than 10%. So, performance is comparable in almost all cases, and the difference is small enough not to be noticeable. The numbers do not, however, indicate that NFS is better than Fibre Channel. You can read my views on [this storage protocol comparison][3] at my site. By the way, also check the comments; you'll see that the results in the technical report were independently verified by VMware as well. Based on this information, someone could certainly say that NFS performance is perfectly reasonable, but one could not say that NFS performance is better than Fibre Channel.

Now, one might look at this article and say, "Scott, you hate NFS!" No, actually, I _like_ using NFS for VMware Infrastructure implementations, and here's why:

* Provisioning is a breeze. It's dead simple to add NFS datastores.

* You can easily (depending upon the storage platform) increase or decrease the size of NFS datastores. Try decreasing the size of a VMFS datastore and see what happens!

* You don't need to deal with the complexity of a Fibre Channel fabric, switches, WWNs, zones, ISLs, and all that. Now, there is some complexity involved (see Myth #2 above), but it's generally easier than Fibre Channel. Unless you're a Fibre Channel expert, of course...

So there _are_ some tangible benefits to using NFS for VMware Infrastructure. But let's be real about this, and not try to gloss over technical details. While NFS has some real advantages, it also has some real disadvantages, and organizations choosing a storage protocol need to understand both sides of the coin.

[1]: {{< relref "2008-03-31-only-thin-provisioned-in-the-beginning.md" >}}
[2]: {{< relref "2008-07-16-understanding-nic-utilization-in-vmware-esx.md" >}}
[3]: {{< relref "2008-08-14-storage-protocol-performance-whitepaper-from-netapp.md" >}}
