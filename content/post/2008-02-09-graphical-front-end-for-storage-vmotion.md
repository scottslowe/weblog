---
author: slowe
categories: Information
comments: true
date: 2008-02-09T09:16:24Z
slug: graphical-front-end-for-storage-vmotion
tags:
- Storage
- Virtualization
- VMware
title: Graphical Front-End for Storage VMotion
url: /2008/02/09/graphical-front-end-for-storage-vmotion/
wordpress_id: 628
---

My [dislike for the Remote CLI][1] is fairly well-known. Unfortunately, using the Remote CLI is the only way to perform a Storage VMotion. Storage VMotion is, of course, the new feature in ESX Server 3.5 that allows for a running VM's virtual disk files to be relocated from one datastore to a second datastore without any service interruption. A very handy feature, indeed, but hobbled---in my opinion---by its dependence upon the Remote CLI.

However, an enterprising developer has written a graphical front-end for the operation. Numerous sites have discussed it; for example, see [Eric Sloof's post](http://www.ntpro.nl/blog/archives/366-SVMotion-graphical-user-interface.html) or [Anders' post](http://www.amikkelsen.com/?p=58) (and the referenced [VMware Communities thread](http://communities.vmware.com/thread/122847?tstart=0)). I'm sure there have been numerous other sites as well.

Now, if a single developer could write a GUI for Storage VMotion, why didn't VMware take care of this themselves?

[1]: {{< relref "2008-01-07-underwhelmed-by-the-remote-cli.md" >}}
