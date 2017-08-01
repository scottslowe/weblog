---
author: slowe
categories: News
comments: false
date: 2006-06-04T21:44:53Z
slug: strataguard-free
tags:
- Linux
- Networking
- OSS
- Security
- Virtualization
- VMware
title: StrataGuard Free
url: /2006/06/04/strataguard-free/
wordpress_id: 260
---

In the next few days, I'm going to try out [StrataGuard Free](http://www.stillsecure.org/), a freeware version of StrataGuard, a [Snort](http://www.snort.org/)-based IDS/IPS. [StillSecure](http://www.stillsecure.com/) is making StrataGuard Free available as a [VMware](http://www.vmware.com/) image for easy testing.

StrataGuard Free is rate-limited, meaning it only handles traffic streams up to 5Mbps. Of course, that is more than sufficient for small businesses and home offices, and it's a great way to become more familiar with the commercial product ([StrataGuard](http://www.stillsecure.com/strataguard/index.php))---which, of course, is not rate-limited and offers more features (such as automated rule updates).

I'm going to be trying out the VMware appliance on [ESX Server](http://www.vmware.com/products/esx/) in my test lab; hopefully, I won't run into any hardware issues. (Remember that my test of [FreeNAS](http://www.freenas.org/), which was also packaged as a VMware image, [did not work on ESX Server][1] due to a SCSI adapter issue.) As soon as I get it up and working (or don't), I'll post more information here.

Thanks to [DABCC](http://www.dabcc.com/dabcc/webapplication/aspx/dabcc.content.aspx?intPKText=2020&intPKChannel=13) for alerting me to the release of StrataGuard Free as a VMware image.

[1]: {{< relref "2006-04-17-freenas-on-esx-server.md" >}}
