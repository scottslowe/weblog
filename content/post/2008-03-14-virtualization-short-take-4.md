---
author: slowe
categories: Information
comments: true
date: 2008-03-14T09:40:31Z
slug: virtualization-short-take-4
tags:
- Citrix
- Microsoft
- NetApp
- Security
- Snapshots
- Storage
- VDI
- Virtualization
- VMware
- Xen
title: 'Virtualization Short Take #4'
url: /2008/03/14/virtualization-short-take-4/
wordpress_id: 660
---

Once again, here's my take a few virtualization-related stories that have passed through my computer in the last few days:

* OK, this first one isn't technically related to virtualization, but it was too good to pass up. Is there anyone besides me and [The Register](http://www.theregister.co.uk/2008/03/12/netapp_novatio_logo/) who thinks NetApp's new logo is...um...well, not as good as the previous one?

* A [new blog war](http://www.dabcc.com/article.aspx?id=7285) is brewing between VMware and Citrix, and this time I had nothing to do with it: VMware apparently launched the first volley in discussing the [value of ESX Server's memory overcommitment and page sharing](http://blogs.vmware.com/virtualreality/2008/03/cheap-hyperviso.html) functionality; Citrix's Roger Klorese [then responded](http://community.citrix.com/display/~rogerkl/2008/03/12/Memory+Lapse) and Simon Crosby [chimed in as well](http://community.citrix.com/pages/viewpage.action;jsessionid=aoSuPNZNwfvflXzvTI?pageId=21792124). I would completely agree with Roger's and Simon's comments, except for this one statement in Eric's original post:  

	>We created and powered on 512MB Windows XP VMs **_running a light workload_** [emphasis mine] and kept adding them until the server couldnt take any more.

	Since Eric stated the parameters of the test involved lightly loaded workstations, Roger's comments about heavy workloads don't apply. Besides, any engineer worth his/her weight isn't going to overcommit a production workload like that, and [this analysis](http://vmmba.com/2008/01/03/why-does-oversubscription-matter.aspx) shows that some overcommitment can produce notable financial results.

* CIO Magazine recently published a [list of 10 virtualization risks hiding in your company](http://advice.cio.com/laurianne_mclaughlin/top_ten_virtualization_risks_hiding_in_your_company). It's a pretty interesting list, although it's worthwhile to note that this list was produced by a VP of Marketing for Embotics and therefore is heavily slanted toward the risks that his company's products can help mitigate.

* [This is](http://www.lostcreations.com/code/wiki/vmware/viplugins/37migrations) [interesting](http://37migrations.com/) and novel, but that's about it. (**UPDATE:** The creator of the 37migrations VI plugin, Schley Andrew Kutz, wrote me to state that there is no point in 37migrations; it's just for fun. So stop trying to find a deeper meaning in it, OK?)

* There's [apparently a problem](http://virtualizationinformation.com/?p=28) with using Sysprep in VirtualCenter 2.5 with Windows Server 2003 SP2. A Microsoft hotfix is available.

* Speaking of NetApp, they've been [generating some buzz](http://blogs.netapp.com/storage_nuts_n_bolts/2008/03/sneak-preview-1.html) around their SnapManager for Virtual Infrastructure (SMVI) product, yet [another unreleased product][1]. I echo [Duncan's thoughts](http://www.yellow-bricks.com/2008/03/11/snapshot-manager-for-vi3-netapp/) about the VC plugin!

* Gabe shares some information he's [gathered about VMsafe](http://www.gabesvirtualworld.com/?p=58), the recently announced security APIs from VMware.

* Alessandro [shares his thoughts](http://www.virtualization.info/2008/03/giant-is-moving-new-microsoft-360.html) about Microsoft's virtualization strategy following the announcement of [Microsoft's purchase of Kidaro][2]. My question is this: was VMware's [announcement of offline VDI functionality][3] at VMworld Europe 2008 because they had an inkling of Microsoft's moves, or is Microsoft's purchase a result of VMware's announcement?

That's it for today. Join in the discussion by adding your 2 cents in the comments below!

[1]: {{< relref "2008-02-28-i-love-it-but-its-not-available.md" >}}
[2]: {{< relref "2008-03-12-microsoft-buying-kidaro.md" >}}
[3]: {{< relref "2008-02-26-vdi-announcements-at-vmworld-europe-2008.md" >}}
