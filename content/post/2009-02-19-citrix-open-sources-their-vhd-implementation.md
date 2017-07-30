---
author: slowe
categories: News
comments: true
date: 2009-02-19T18:55:02Z
slug: citrix-open-sources-their-vhd-implementation
tags:
- Citrix
- HyperV
- Virtualization
- Xen
title: Citrix Open Sources Their VHD Implementation
url: /2009/02/19/citrix-open-sources-their-vhd-implementation/
wordpress_id: 1205
---

One sticking point I've had with Citrix since the XenSource acquisition has been the perception of a failure to give back to the open source Xen community. Note that I said _perception_. It appeared, following the XenSource acquisition, that Citrix was all about using open source Xen as a base but failing to return any of enhancements they made to the code base. No, I don't have any concrete examples; again, this was the _perception._

It appears that Citrix is now taking steps to remedy that perception. In a blog entry posted last night, Simon Crosby announced that Citrix has [open sourced their optimized VHD support](http://community.citrix.com/blogs/citrite/simoncr/2009/02/18/We%27ve+Open+Sourced+Our+Optimized+VHD+Support). This means that XenServer's robust VHD implementation is now available to any developer under the BSD license. In case you don't already know, VHD is the same virtual disk technology Microsoft uses in Hyper-V, and which Microsoft is using even more extensively in future versions of Windows.

In my opinion, this is an excellent move. It addresses the perception of failing to give back to the open source community, and it puts what appears to be a valuable piece of technology into the open source world. Making XenServer's VHD implementation available for other open source developers to use in their projects puts VHD on the fast track to being the _de facto_ virtual disk standard. Assuming that other virtualization platforms adopt VHD support---and I'm not sure why many of them wouldn't adopt VHD support, except for VMware---we've now removed a huge barrier to interoperability. That's a good thing.

Not being a lawyer, I'm a bit worried about the compatibility of the BSD license---which is generally regarded as quite generous---and the [Microsoft Open Specification Promise](http://www.microsoft.com/Interop/osp/default.mspx), but I'll leave that for others to hash out.

It will be interesting to see if Citrix also open sources some of their other XenServer-related technologies. Time will tell...
