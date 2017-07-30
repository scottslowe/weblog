---
author: slowe
categories: Information
comments: true
date: 2008-04-29T22:54:08Z
slug: virtualization-short-take-6
tags:
- ESX
- HyperV
- Microsoft
- Virtualization
- VMware
title: 'Virtualization Short Take #6'
url: /2008/04/29/virtualization-short-take-6/
wordpress_id: 698
---

This one is a short one, since I'm out of town on business and don't have a great deal of time. Nevertheless, I wanted to post my thoughts on a couple of items that have passed in front of me over the last few days.

* Andrew Kutz has released the [VI Toolkit for .NET](http://code.lostcreations.com/wiki/vmware/vitoolkitfordotnet). Quoting from the e-mail he sent me, the VI Toolkit for .NET "makes connecting to VI, finding a VM, and getting its properties a snap!". If you're a developer that is interested in developing tools and/or scripts for the VI environment, this seems like it would be a tool that would be useful.

* VKernel let me know that they've joined the VMware Certified Virtual Appliance program with their Chargeback Virtual Appliance.

* Rich over at VM /ETC has published some information on [using the VI Client to upgrade VMware Tools](http://vmetc.com/2008/04/29/use-the-vi-client-to-bulk-upgrade-vm-tools/). The particularly helpful portion of this article is the information on using the CLI from the VirtualCenter server to upgrade VMware Tools.

* Duncan shares some information from one of his readers about a [bug in the VMware Tools for Solaris 10](http://www.yellow-bricks.com/2008/04/29/vcb-and-solaris-32-bit-vms/). This particular issue can prevent VCB from working properly. Fortunately, Duncan also shares a workaround.

* Via Alessandro, it looks like Microsoft is upping the heat on VMware with [the release of a beta of SCVMM 2008](http://www.virtualization.info/2008/04/microsoft-opens-scvmm-2008-beta-manages.html). One of the most intriguing features to be included in the next version is management from SCVMM of VMware ESX Server. When you're a company like Microsoft, you can afford to wage a battle on two fronts---in this case, on both the hypervisor front and the hypervisor management front.

* Again via Duncan, it looks like there's an [error with ESX 3.5 Update 1](http://www.yellow-bricks.com/2008/04/28/pegasus-error-after-installing-esx-35-update-1/) with the Pegasus installation. I ran into this error myself when installing Update 1 right after it was released, but then marked it up as a symptom of the whole snafu with the wrong version of the ISO being posted. Looks like it was more than that.

* Rich has some [good information on ESX-VirtualCenter compatibility](http://vmetc.com/2008/04/28/determine-esx-and-virtualcenter-version-compatibility/). (Yes, I do read more blogs than just Rich's and Duncan's. Honest!)

* OK, this may not be necessarily virtualization related, but a pretty good storage-related blog, which does address some VMware-specific issues, has sprouted up [here](http://21stcenturystorage.cebis.net/).

* SearchVMware.com has published an interesting article by David Davis about [using ESX to help with an Exchange migration or Exchange upgrade](http://searchvmware.techtarget.com/tip/0,289483,sid179_gci1310729,00.html?track=NL-923&ad=635967&asrc=EM_NLN_3523276&uid=1425534).

* If you have any questions about networking performance on ESX, just check out [this document](http://www.vmware.com/resources/techresources/1041?elq=32D28E76B9154C9AA242D21F81277A72), last revised close to the end of February of this year.

Well, that's it for now. Thanks for reading!
