---
author: slowe
categories: Information
comments: false
date: 2006-07-03T19:46:49Z
slug: group-policy-resources
tags:
- ActiveDirectory
- Microsoft
- Windows
title: Group Policy Resources
url: /2006/07/03/group-policy-resources/
wordpress_id: 290
---

I just posted several links for Group Policy-related web resources to [my del.icio.us bookmarks list](http://del.icio.us/slowe) (subscribe to [my del.icio.us RSS feed](http://del.icio.us/rss/slowe) for regular updates). There are some good sites included, as well as some pretty handy tools.  Since I just landed a big AD migration project, I have a feeling some of these tools may come in handy.

Here are the links that I just added:

[Group Policy Wiki](http://grouppolicy.editme.com/)  
[GPOGuy.com - The Windows Group Policy Information Hub](http://www.gpoguy.com/)  
[FirefoxADM](http://sourceforge.net/projects/firefoxadm)  
[SpecOps GPUpdate](http://www.specopssoft.com/products/specopsgpupdate/Default.asp)

Some of these are pretty straightforward, like the link to the ADM template for Firefox (this is different from the earlier [Firefox-Group Policy stuff][1] I mentioned a short while back).  The SpecOps GPUpdate utility, for remotely forcing an update of Group Policy settings, is more interesting; I'm particularly interested in how it can be scripted/automated to affect large numbers of computers at a time.

If you know of any other useful Group Policy resources, send the link to [del.icio.us](http://del.icio.us/) and mark it "for:slowe" or add it in the comments.

**UPDATE:** I tested the [SpecOps GPUpdate](http://www.specopssoft.com/products/specopsgpupdate/Default.asp) product (or tried to) late last week, but was utterly and completely unable to make it even launch. I'm not sure what I did wrong, if anything, since it was a simple Windows Installer package that didn't really ask any questions except where to place its files. Even so, it absolutely refused to launch, so I have no idea if this product is any good or even worth investigating.

[1]: {{< relref "2006-06-09-extending-group-policy.md" >}}
