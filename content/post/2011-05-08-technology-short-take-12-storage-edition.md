---
author: slowe
categories: Information
comments: true
date: 2011-05-08T23:34:03Z
slug: technology-short-take-12-storage-edition
tags:
- CLARiiON
- EMC
- FCoE
- SAN
- Storage
title: 'Technology Short Take #12: Storage Edition'
url: /2011/05/08/technology-short-take-12-storage-edition/
wordpress_id: 2291
---

I'm so far behind in my technology reading that I have this massive list of blog posts and links that I would normally put into an issue of Technology Short Takes. However, people are already "complaining" that my Short Takes aren't all that short. To keep from overwhelming people, I'm breaking Technology Short Take #12 into three editions: Virtualization, Storage, and Networking.

Here's the "Storage Edition" of Technology Short Take #12!

* When planning storage details for your vSphere implementation, be sure to keep block size in mind. Duncan Epping's post on [the performance impact of the different datamovers](http://www.yellow-bricks.com/2011/02/24/storage-vmotion-performance-difference/) in a Storage vMotion operation should bring to light why this is an important storage detail to remember. (And read [this post](http://www.yellow-bricks.com/2011/02/18/blocksize-impact/) if you need more info on the different datamovers.)

* Richard Anderson of EMC (aka [@storagesavvy](http://twitter.com/storagesavvy)) posted a "what if" about [using cloud storage as a buffer with thin provisioning and FAST VP](http://storagesavvy.com/2011/02/24/auto-tiering-cloud-storage-and-risk-free-thin-pools/). It's an interesting idea, and one that will probably see greater attention moving forward.

* Richard also shared some [real-world results](http://storagesavvy.com/2011/03/26/real-world-emc-fastvp-and-fastcache-results/) on the benefits of using FAST Cache and FAST VP on a NS-480 array.

* Interested in using OpenFiler as an FC target? Have a look [here](http://brianshowto.com/?p=38).

* Nigel Poulton posted an analysis of EMC's recent entry in the SPC benchmarketing wars in which he [compares storage benchmarking to Formula 1 racing](http://blog.nigelpoulton.com/storage-benchmarking-and-formula-1/). I can see and understand his analogy, and to a certain extent he has a valid point. On the other hand, it doesn't make sense to submit a more "mainstream" configuration if it's a performance benchmark; to use Nigel's analogy, that would be like driving your mini-van in a Formula 1 race. Yes, the mini-van is probably more applicable and useful to a wider audience, but a Formula 1 race is a "performance benchmark," is it not? Anyway, I don't know why certain configurations were or were not submitted; that's for far more important people than me to determine.

* Vijay (aka [@veverything](http://twitter.com/veverything) on Twitter) has a good [deep dive on EMC storage pools](http://virtualeverything.wordpress.com/2011/03/05/emc-storage-pool-deep-dive-design-considerations-caveats/) as implemented on the CLARiiON and VNX arrays.

* Erik Smith has a couple of great FCoE-focused blog posts, first on [directly connecting to an FCoE target](http://brasstacksblog.typepad.com/brass-tacks/2011/03/pt2pt-and-vn2vn-directly-connecting-an-fcoe-initiator-to-an-fcoe-target-part-1-overview.html) and then on [VE_Ports and multihop FCoE](http://brasstacksblog.typepad.com/brass-tacks/2011/03/fcoe-multihop-and-ve_ports.html). Both of these posts are in-depth technical articles that are, in my opinion, well worth reading.

* Brian Norris posted about [some limitations with certain FLARE 30 features](http://goingvirtual.wordpress.com/2011/02/17/celerra-dart-6-0-x-with-clariion-flare-30-gotcha/) when used in conjunction with Celerra (DART 6.0). I know that at least one of these limitations---the support for FAST VP on LUNs used by Celerra---are addressed in the VNX arrays.

* Brian also recently posted some good information on [a potential login issue with Unisphere](http://goingvirtual.wordpress.com/2011/02/28/celerra-and-clariion-login-gotcha-with-unisphere/); this is caused by SSL certificates that are generated with future dates.

* J Metz of Cisco also has a couple of great FCoE-focused posts. In [To Tell the Truth: Multihop FCoE](http://blogs.cisco.com/datacenter/to-tell-the-truth-multihop-fcoe/), J covers in great detail the various topology options and the differences in each topology. Then, in his post on [director-class multihop FCoE](http://blogs.cisco.com/datacenter/announcing-true-director-class-multihop-fcoe/), J discusses the products that implement multihop FCoE for Cisco.

* If you've never used EMC's VSI (Virtual Storage Integrator) plug-in for vCenter Server, have a look at [Mike Laverick's write-up](http://www.rtfm-ed.co.uk/2011/03/01/using-the-emc-vsi-plug-in/).

OK, that does it for the Storage Edition of Technology Short Take #12. Check back in a couple of days for the Networking Edition of Technology Short Take 12.
