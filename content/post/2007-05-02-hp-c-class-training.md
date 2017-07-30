---
author: slowe
categories: Information
comments: true
date: 2007-05-02T09:58:19Z
slug: hp-c-class-training
tags:
- Hardware
- HP
- Networking
- SAN
title: HP c-Class Training
url: /2007/05/02/hp-c-class-training/
wordpress_id: 449
---

I'm in training this week in Canada for the HP c-Class BladeSystem. That means two things: 1) I was unable to attend VMware TSX 2007 in Las Vegas (bummer!), and 2) blog posts may be a bit less frequent than normal. (Of course, I don't really have a "normal" blog posting routine, but you get the idea.)

The HP c-Class BladeSystem is HP's latest blade offering, replacing the earlier p-Class enclosure and blades with a new enclosure (10U instead of 6U), new blades (both half-height and full-height blades, up to 16 half-height blades per enclosure), new interconnects (both Ethernet and SAN), and an entirely redesigned internal architecture. In my opinion, it's a pretty impressive offering.

With this new BladeSystem, HP offers the following:

* Up to 16 half-height blades (the BL460c or BL465c) in 10U of rack space; each half-height blade can go up to dual socket/dual core configurations

* Up to 8 full-height blades (the BL480c, the BL685c, or the BL860c) in 10U of rack space; the BL480c offers dual socket/dual core (and quad core) configurations; the BL685c offers quad socket/dual core configurations. (The BL860c is an Itanium-based blade.)

* Mix and match full-height/half-height blades within an enclosure as needs dictate (with a few caveats, of course)

* A variety of interconnect options for both Ethernet (including a Cisco switch that runs IOS---great for guys like me who are familiar with IOS) and Fibre Channel (both Cisco MDS and Brocade SAN switches are available)

* A variety of mezzanine expansion options that add capabilities to the blades, like additional NICs or Fibre Channel connectivity

With all these options and all the various ways these pieces can be put together, planning and deploying one or more of these enclosures really demands some professional services assistance. That's great for guys like me who work in the professional services arena, but it's going to take some work to get customers to realize that this isn't the typical server deployment. Yes, the blade servers themselves are very similar to your typical rack-mount server, but when you combine the blade servers with the interconnects and the mezzanine cards---the sum of the whole becomes more complex than the individual parts.

It's also going to place more of a burden on the sales team to properly position solutions based around the c-Class BladeSystem to customers. In some ways, that will be a greater challenge than dealing with the customers. (Those of you that work in technical pre-sales roles will appreciate that comment.)

This generation of the blade enclosures and the blade servers themselves also addresses one key concern I had with earlier generations and using them as a virtualization platform: a lack of NICs. With the c-Class, I can now provision quad socket/dual core AMD Opteron-based server blades (the BL685c) with up to 12 NICs (four onboard plus two quad-port NICs in the mezzanine slots), and _still_ have room for Fibre Channel connectivity. That's pretty impressive and makes for quite a platform for VMware ESX Server, if you ask me.

I'll post more information here as the class progresses. If anyone else has any experience (good or bad) with the c-Class enclosures, I'd love to hear about it in the comments.
