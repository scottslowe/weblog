---
author: slowe
categories: Liveblog
comments: true
date: 2009-09-01T11:08:13Z
excerpt: This is a liveblog of the keynote from Day 1 of VMworld 2009.
slug: vmworld-2009-day-1-keynote
tags:
- Virtualization
- VMware
- VMworld2009
title: VMworld 2009 Day 1 Keynote
url: /2009/09/01/vmworld-2009-day-1-keynote/
wordpress_id: 1575
---

_(Note: You can also follow my Twitter updates [here](http://twitter.com/scott\_lowe).)_

I start out the Day 1 keynote at VMworld 2009 at the bloggers' table, sitting with folks like Rodney Haywood, Mike Laverick, Alessandro Perilli, Rich Brambley, Thomas Bryant, Bob Plankers, and---of course---the ever-present John Troyer. There should be some good coverage of the event. Later today, I'll update this post with links to the coverage from some of the other bloggers here.

The keynote session starts off with Todd Nielsen, VMware's Chief Operating Officer (COO). He recounts the story of how he challenged the partners at Partner Exchange to win over one of the 40 customers in the Fortune 1000 for a free pass to the conference. It turns out that 10 of those 40 customers did switch to VMware, so there are now only 30 companies left in the Fortune 1000 that don't use VMware software.

Mr. Nielsen then goes on to recognize the conference sponsors and the lab sponsors.

The VMworld 2009 conference has 12,488 attendees. That's up quite a bit from the estimated attendance of only 8,000 attendees. It's also funny how marketing and sales like's to up those sorts of numbers ("It's almost 15,000!")

Mr. Nielsen goes on to talk about how VMware is driving cost savings (optimize financial energy), shift human energy, and save the Earth's energy (via power reduction). Together these help VMware "energize business through IT." Spend more money on innovation, and less money on maintenance. This has been a key statement in VMware's marketing over the last few months.

Now Paul Maritz takes the stage. He starts out by echoing Mr. Nielsen's comments about needing to spend more on innovation instead of spending on maintenance. Mr. Maritz believes that business agility depends upon IT agility, and IT agility depends upon virtualization.

Mr. Maritz now moves the discussion to---what else?--the idea of cloud computing. He believes that virtualization is a key enabler, a key component, of cloud computing. It allows organizations to insert "cloud mojo" into their data centers. VMware vSphere is the product that provides the ability for organizations to transform their data centers and create private clouds. Once an organization has built their private cloud, VMware vCloud creates the bridge to external clouds.

The virtualization journey starts with CapEx savings and server consolidation, continues on to OpEx savings and internal/external cloud, and finishes with IT agility and business agility. According to Mr. Maritz, the release of VMware vSphere is a bigger release than any Windows release on which he worked while at Microsoft. (Is that a good thing?)

Next, Mr. Maritz reviews the vSphere architecture and discusses the new features and functionality that have been added to the product in this release. This includes DPM, new storage optimizations (more than 350,000 IOPS from a single server), new networking, etc. He reiterates: "There is now literally no application that cannot be considered a candidate for virtualization."

So what is VMware doing with this great virtualization layer? VMware is introducing a family of new products that address specific usage scenarios. Mr. Maritz is referring to the vCenter family of products; not only vCenter Server, but Chargeback, AppSpeed, etc. His initial focus on vCenter is around the ability of integrate vCenter with other products via APIs. To that point, Mr. Maritz brings out an IBM representative to discuss how IBM is leveraging vCenter APIs to provide integration. This is illustrated with a demonstration of how IBM's power meter, embedded in the IBM hardware, has been integrated with vCenter Server.

In the demonstration, IBM shows how their power meter is reporting power usage back to vCenter Server. This information is being reported not only for the physical hosts, but power is being reported for virtual machines as well. (I wonder upon which metrics IBM is basing virtual machine power usage?)

Following the demonstration, Mr. Maritz moves on to some new members of the vCenter family: capacity planning (CapacityIQ), configuration (ConfigControl), operations (Chargeback?), and continuity (SRM). Additionally, VMware is introducing products or technologies to create service profiles, service catalogs, enable self-service, and further extend chargeback functionality. In addition, VMware will be focusing on increasing app visibility.

This leads into another demonstration, this time with Bruce of VMware. Bruce is going to be demonstrating Lab Manager 4.0, which is enabling the self-service aspect of the private cloud. Next up Bruce and Mr. Maritz walk through a demonstration of vCenter Chargeback.

The discussion moves on now to vSphere Essentials, which provides "IT in a box" for smaller organizations. VMware is extending their push into the SMB market with VMware Go, a new service being introduced by VMware this week at the conference. VMware Go is a set of web-based services targeted at the SMB customers that helps automate the installation of VMware ESXi and helps engage partners and the virtualization community.

The next stage of the virtualization journey is bridging the connection to the external cloud. This leads into a discussion of VMware vCloud and vCloud Express. To help make this possible, VMware is adding a new construct called the "virtual datacenter". The virtual datacenter allows organizations to amalgamate internal and external clouds for management, provisioning, resource allocation, etc. A key to making this work is having external providers ready to work with organizations and VMware to enable them to slide virtual datacenters from the internal cloud to the external cloud. (This sounds great and wonderful, but I think there are a lot of challenges yet to be addressed before this becomes reality.)

Mr. Maritz at least recognizes that organizations must have the ability to migrate out of the cloud as well as into the cloud. That's better than some people I hear talking about cloud computing.

vCloud Express is targeted at providing the ability to quickly and cost-effectively turn up virtualized resources, targeting the sort of things that organizations can do today with Amazon. Bruce comes back out again for a demo of vCloud Express, created in conjunction with VMware's partner Terremark. (vCloud Express looks interesting, although I'm not sure how excited companies will be about the ability for their employees to go create new virtual machines with an external provider for "only $20-30 per month".)

Today VMware is formally announcing the vCloud API. This is something that VMware has been discussing for quite some time. In addition, this API is being submitted to standards organizations (the DMTF, I believe).

Another part of VMware's "virtualization journey" involves desktop virtualization with VMware View. To help further discuss VMware View and how desktop virtualization is evolving, Mr. Maritz invites out Todd Dupree from HP. (While I love HP, this part is mostly a commercial. Bummer.)

Todd from HP does, at least, take a few minutes to discuss a new product from HP, Insight Control for VMware View. This is reasonably interesting and relatively well-integrated into vCenter Server and the vSphere Client, and provides hooks back into other HP products and management consoles.

PCoIP (PC over IP) is key component of ubiquitous desktop virtualization. To help illustrate that, Mr. Maritz next invites Chris Renter, of TELUS Communications in Canada. Mr. Renter logs into a virtual desktop using the next version of VMware View---presumably using PCoIP---to provide a short commercial about TELUS and how TELUS is using VMware View.

Mr. Maritz now moves into a discussion of the SpringSource acquisition, as a "final step" in the virtualization journey. Why is the SpringSource acquisition important? VMware believes that the use of lightweight frameworks and virtualization will result in "radical simplification" and will help eliminate redundant and complex layers of management. Mr. Maritz reiterates the commitment to keep the Spring framework as an open source project, and that the Spring framework will continue to support multiple platforms and Java services. What will VMware do with SpringSource? The idea is to find ways to slide the Spring framework onto VMware vSphere and enable ways for Spring to inform the hypervisor about application requirements and behaviors. To help show this off, Mr. Maritz invites Rod Johnson to take the stage and show off some of the SpringSource functionality.

My next session starts in 5 minutes, so I'm packing it up now and moving on. I'll update this post later with links to other bloggers' coverage of the keynote.

**UPDATE:** Here are some additional links with coverage of the Day 1 keynote:

[Live from VMworld 2009 - Day 1](http://blog.dynatrace.com/2009/09/01/live-from-vmworld-2009-day-1/)  

[VMworld Day 1: Tuesday: The Keynote](http://www.rtfm-ed.co.uk/?p=1683)  

[Live from VMworld 2009: Day 1](http://www.virtualization.info/2009/09/live-from-vmworld-2009-day-1.html)  

[VMworld 2009 keynote - day one](http://virtualization.com/news/2009/09/01/vmworld-2009-keynote-day-one/)  

[VMworld 2009 (San Francisco) Day 1: Key Note - A Summary](http://www.techhead.co.uk/vmworld-2009-san-francisco-day-1-key-note-a-summary)  

[VMworld 2009 Keynote Day 1](http://vmguy.com/wordpress/index.php/archives/1048)  

[Data Center Strategies: VMworld Day 1 Keynote A Few Thoughts](http://dcsblog.burtongroup.com/data_center_strategies/2009/09/vmworld-day-1-keynote-a-few-thoughts.html)

Readers should also refer to [Planet V12n](http://www.vmware.com/vmtn/planet/v12n/) for more information as well.
