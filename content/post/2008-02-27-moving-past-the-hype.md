---
author: slowe
categories: Rant
comments: true
date: 2008-02-27T09:42:38Z
slug: moving-past-the-hype
tags:
- ESX
- Security
- Virtualization
- VMware
title: Moving Past the Hype
url: /2008/02/27/moving-past-the-hype/
wordpress_id: 646
---

It's only natural, I suppose. When wireless networking started to become popular, it was decried as insecure and everyone was warned against using it. When mobile computing started to take off, it was proclaimed a terrible security risk, and organizations were warned against it. And now it's happening with server virtualization. Of course, since VMware is the lead player in this realm, they are the ones with the target on their back.

The latest volley comes from a "one-two" punch over the past couple of weeks. First, there was a [vulnerability discovered](http://isc.sans.org/diary.html?storyid=4018) in some of the VMware hosted client products (VMware Workstation and VMware Player); specifically, when using the Shared Folders feature. This feature allows host-to-guest interaction. The press went crazy with this one:

* [Critical VMware bug lets attackers zap 'real' Windows](http://www.computerworld.com/action/article.do?command=viewArticleBasic&articleId=9064319&source=rss_news50)

* [New VMware vulnerability without a patch](http://blogs.ittoolbox.com/security/adventures/archives/new-vmware-vulnerability-without-a-patch-22672?rss=1)

Fortunately, some level of clarity has started to prevail about this flaw:

* [VMware Security Crumbling: Not Really](http://secauditor.wordpress.com/2008/02/26/vmware-security-crumbling-not-really/)

* [VMware getting a lot of bad press](http://techdulla.wordpress.com/2008/02/26/vmware-getting-a-lot-of-bad-press/)

The other "flaw" that's gotten a fair amount of attention---and hype---is the exploit that can affect VMs during a live migration. Of course, this assumes that necessary steps weren't taken to protect and isolate the live migration network, as [recommended by VMware](http://blogs.vmware.com/security/2008/02/keeping-your-vm.html). I won't spend a great deal of time on this, since [Chris Hoff already said](http://rationalsecurity.typepad.com/blog/2008/02/news-flash-if-y.html) pretty much everything that needs to be said.

So what's the takeaway from all this? Basically, exactly what Chris Hoff said: Don't be surprised that your installation is insecure when you haven't taken the time to implement the correct security controls. If you configure guest-to-host connectivity, then of course you open a channel for some sort of exploit. Best practices would recommend _not_ to configure guest-to-host connectivity. Likewise, if you run the VMotion (or XenMotion) network shared with other traffic, you run the risk of VM state being exposed.

Let's move past the hype. Just take the time to do your due diligence, pay attention to the security risks of the choices you're making, and don't blame the vendor when you don't follow the vendor's security recommendations and get an insecure result.
