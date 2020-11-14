---
author: slowe
categories: Rant
comments: true
date: 2007-08-29T11:28:25Z
slug: swsoft-disagrees-with-cnet-benchmarks
tags:
- macOS
- Virtualization
- VMware
title: SWsoft Disagrees with CNET Benchmarks
url: /2007/08/29/swsoft-disagrees-with-cnet-benchmarks/
wordpress_id: 515
---

Via David Marshall over at [VMblog.com](http://vmblog.com/), I picked up this article about [SWsoft disagreeing with some CNET benchmarks](http://virtuozzoblog.swsoft.com/2007/08/parallels-desktop-benchmarked.html). The benchmarks in question were comparing [VMware Fusion](http://www.vmware.com/mac/), [Parallels Desktop](http://www.parallels.com/en/products/desktop/) (keep in mind that [SWsoft](http://www.swsoft.com/) owns Parallels, if you didn't already know that), [CrossOver Mac](http://www.codeweavers.com/products/cxmac/), and Boot Camp.

Intrigued, I reviewed the referenced blog post by SWsoft. Their disagreements basically boil down to these key points:

* CNET should not have used an 8-core Mac Pro, citing that "people use laptops with 2 CPU cores and 2GB of RAM".

* CNET should have not provisioned the VMs (where possible) with multiple virtual CPUs.

* CNET should not have used Vista inside the VMs.

* CNET should not have used Windows applications inside the VMs when there are Macintosh equivalents. Specifically, he mentions QuickTime and Photoshop.

As an aside, it appears that Ilya (the Director of Technology at SWsoft and the author of the article disagreeing with the CNET benchmarks) also has a problem with VMmark, stating that it is "designed to work best with ESX and not work at all with Virtuozzo". Well, yes, that is true, since as I understand it VMmark was _designed to benchmark hardware performance on ESX Server **not** provide cross-vendor virtualization comparisons_. (If there are any VMmark team members reading, by chance, feel free to correct me there.) Anyway, back to the CNET report...

My interest is now piqued; some of his concerns seem valid. So I head over to have a look at the [CNET article directly](http://crave.cnet.com/8301-1_105-9760910-1.html?tag=nefd.lede). After reviewing the CNET article, I quickly came to the conclusion that Ilya's concerns are mostly unfounded. Here's why.

1. First, Ilya casts the CNET article as a direct performance comparison between VMware Fusion and Parallels Desktop. It's not. The article is much more focused on comparing the performance of various virtualization solutions against native Mac applications themselves. If the article were strictly focused on virtualization performance _per se_, why include native Mac results at all?

2. Keeping #1 in mind, the use of applications such as QuickTime and Photoshop makes sense. If you want to compare the performance of an aplication under a virtualized solution, you'll need a baseline against which to compare it, i.e., a native version of the same application.

3. At least one of the tests was specifically intended to test multi-CPU performance, hence the need to provision a VM with multiple virtual CPUs.

There are some valid concerns---the use of Vista vs. Windows XP, for example, is a valid complaint. And yes, CNET probably should have chosen a better hardware platform that is more representative of the typical user of one of these virtualization solutions. To be fair, though, I don't think that choosing a different hardware platform would have made any real difference in the comparisons between Fusion and Parallels, instead impacting the comparison between native performance and virtualized performance. If anything, the use of an 8-core Mac Pro unfairly skewed the results in favor of native application performance, casting both Fusion and Parallels in an unfair light.

Even given these concerns, though, I don't think I would have said that "everything about this test is upside down". Could the test have been improved, made more relevant? Yes, probably, but it's important to keep this "comparison" in the context in which it appears it was intended, and that is trying to establish what kind of performance impact is introduced by the virtualization solutions when compared with native performance.
