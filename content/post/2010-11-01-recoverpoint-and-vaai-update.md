---
author: slowe
categories: Information
comments: true
date: 2010-11-01T12:54:34Z
slug: recoverpoint-and-vaai-update
tags:
- EMC
- Interoperability
- Storage
- Virtualization
- VMware
- vSphere
title: RecoverPoint and VAAI Update
url: /2010/11/01/recoverpoint-and-vaai-update/
wordpress_id: 2151
---

Last week I published a quick note about [RecoverPoint-VAAI interoperability][1] that outlined some potential concerns around the use of VAAI with RecoverPoint. In that post---which was based on current information from the RecoverPoint product management team---I called out the need to disable some VAAI functionality because it was our understanding that RecoverPoint ignored certain VAAI commands instead of rejecting them as not implemented. (Rejecting them is actually the preferred behavior, since it forces the VMware ESX/ESXi hosts to fall back to pre-VAAI operation.)

Today I received word that the current version of RecoverPoint (available today) does properly **_reject_** unsupported VAAI commands instead of ignoring them, when used in conjunction with the array splitter out of FLARE 30. (You might initially question the need for the splitter out of FLARE 30, but recall that FLARE 30 is the version needed to support VAAI.) This is good news!

So what does this mean? There are two key takeaways:

1. You do **_not_** need to disable **_any_** VAAI functionality on your VMware ESX/ESXi hosts. With the FLARE 30 array splitter, RecoverPoint will properly reject unimplemented or unsupported VAAI commands.

2. Remember that the current version of RecoverPoint (available right now) **_does_** support hardware-accelerated locking.

I also received confirmation that the next release of RecoverPoint will implement proper rejection of unimplemented or unsupported VAAI commands when used with intelligent fabric splitters. Again, this is good news---it means that you won't have to disable VAAI functionality with fabric splitters with the next release. For the current release, though, you'll still need to disable VAAI functionality with the fabric splitters.

Here's a quick summary, then, of the configurations and the steps required:

* **With the FLARE 30 array splitter:** No additional configuration required. Hardware-accelerated locking is fully supported, and all other commands properly rejected. There is no need to disable VAAI on the VMware ESX/ESXi hosts.

* **With the fabric splitters:** In current release, VAAI commands are ignored, not rejected. You need to disable VAAI on hosts (see [here][2] for information how). The next release will reject VAAI commands properly; at that point, VAAI can be left enabled. Disable VAAI until then.

If you have any questions or comments, please let me know.

[1]: {{< relref "2010-10-28-recoverpoint-and-vaai-interoperability.md" >}}
[2]: {{< relref "2010-10-29-disabling-vaai-using-the-cli.md" >}}
