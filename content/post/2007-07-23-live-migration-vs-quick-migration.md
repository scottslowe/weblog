---
author: slowe
categories: Rant
comments: true
date: 2007-07-23T23:42:47Z
slug: live-migration-vs-quick-migration
tags:
- ESX
- Microsoft
- Virtualization
- VMware
title: Live Migration vs. Quick Migration
url: /2007/07/23/live-migration-vs-quick-migration/
wordpress_id: 493
---

So there's been a flurry of coverage over the last couple of weeks regarding [a statement from Microsoft](http://www.theregister.co.uk/2007/07/11/microsoft_virtualization_roadmap_confusion/) regarding WSV's lack of live migration (aka VMotion) functionality. As I mentioned when I discussed the [announcement of features being cut][1] from Windows Server Virtualization (WSV, or "Viridian"), the lack of live migration functionality really is, in my opinion, a serious stumbling block for Microsoft. I [wasn't the only one](http://h0bbel.p0ggel.org/2007/05/11/microsoft-adjusts-viridian-feature-set/) that thought so, either. (There was a post about it on the [VMTN Blog](http://blogs.vmware.com/vmtn/) at some point as well, but I can't find a link for it now.)

After Microsoft announced that live migration wasn't going to make it into WSV and virtualization-savvy users responded with numerous statements about how this killed WSV's chances against VI3, Microsoft apparently realized that live migration was, in fact, important to customers. (Apparently customers don't like downtime. Who knew?)

To help alleviate the bad press over the lack of live migration in a product that is already pretty far behind the competition, Microsoft now started spewing stuff about _quick migration:_

>"The recent press has been inaccurate to say we don't do migration - we do migration: quick migration," Lees said Wednesday. Live migration is a memory-to-memory system while quick migration is machine-to-machine and disc-to-disc.

Mr. Andy Lees (speaking in the quote above taken from [this article from The Register](http://www.theregister.co.uk/2007/07/11/microsoft_virtualization_roadmap_confusion/)) goes on further to say:

>Lees called the debate between quick migration and live migration a "red herring" based on six seconds of difference, which mattered only to disaster recovery. On that basis, Windows Server 2008's planned geo-clustering feature would help users out of any squeeze and could - he claimed - beat VMware.

Yes, but it's [still downtime](http://virtualscoop.org/?q=node/4), Mr. Lees! That's the point---without live migration, _any_ migration solution still involves downtime, and therefore can't really compete with VMotion. Host-level clustering is great, and something I'd love to see VMware tackle for those shops where VMware HA just isn't quite enough to meet SLAs. But host-level clustering is no substitution for VMotion.

Even [this blog posting](http://blogs.technet.com/daven/archive/2007/06/08/virtual-server-quick-migration.aspx) (from an MS employee, no less) admits that Quick Migration is really nothing more than Host Clustering. This isn't new technology; this is just Microsoft repackaging the same old stuff again. (Unless memory serves me incorrectly, Microsoft has been talking about clustering hosts for a while now.)

Microsoft can change the names of features to make them look more like their competition, but the fact of the matter remains: when Windows Server Virtualization debuts in late 2008 (supposedly within 180 days of the release of [Windows Server 2008](http://www.microsoft.com/windowsserver2008/default.mspx), which is due to be released in late February 2008), it _still_ won't have a feature set comparable to the feature set that VMware has today.

[1]: {{< relref "2007-05-10-viridian-feature-set-trimmed.md" >}}
