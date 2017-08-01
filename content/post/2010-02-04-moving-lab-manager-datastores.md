---
author: slowe
categories: Information
comments: true
date: 2010-02-04T16:14:49Z
slug: moving-lab-manager-datastores
tags:
- Storage
- Virtualization
- VMware
title: Moving Lab Manager Datastores
url: /2010/02/04/moving-lab-manager-datastores/
wordpress_id: 1820
---

You might have noted a slight incompatibility between VMware vCenter Lab Manager and one of VMware vSphere's core features, Storage VMotion, in [this earlier post on VMware Lab Manager design considerations][1] by former co-worker Aaron Delp:

>Storage VMotion and VMware VCB are not supported with Lab Manager.

Obviously, this could present a problem for users who might need to migrate Lab Manager datastores from one LUN or array to another LUN or array. So what's a user to do?

Fortunately, an internal discussion on this earlier today turned up some great information on a utility called SSMove. What is SSMove?

>SSMove is a utility installed on the Lab Manager server that allows you to move data from one datastore to another. You can move a specific tree of related virtual machines. See "Viewing Virtual Machine Datastore Directories" in the _Lab Manager User's Guide_ for more information on trees. To move an entire datastore, you must move all its trees individually.

Credit goes to rockstar team member Denis Guyadeen for pointing out this utility. More information is available at this links:

[VMware KB: Moving a datastore using SSMove](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1006694)  

[VMware KB: SSMove does not work if a datastore is disabled (3.0.2 only)](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1009056)  

[VMware Lab Manager 3 Online Library - Managing Datastores](http://pubs.vmware.com/labmanager3/users/wwhelp/wwhimpl/common/html/wwhelp.htm?context=users&file=LM_Users_Guide_administration.10.20.html)

So, if you are needing to migrate data in Lab Manager from one datastore to another, this is your tool.

I haven't yet found any information on whether SSMove is also included in Lab Manager 4. (To be fair, I haven't really searched too hard.) If anyone knows, please speak up in the comments.

[1]: {{< relref "2009-10-04-vmware-lab-manager-design-considerations.md" >}}
