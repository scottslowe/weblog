---
author: slowe
categories: Liveblog
comments: true
date: 2008-09-16T18:11:16Z
slug: bc1693-architecting-dr-solutions-with-vmware-srm
tags:
- Virtualization
- VMware
- VMwareSRM
- VMworld2008
title: 'BC1693: Architecting DR Solutions with VMware SRM'
url: /2008/09/16/bc1693-architecting-dr-solutions-with-vmware-srm/
wordpress_id: 910
---

Late again! Man, I need to get on the ball! Fortunately, I only missed the first part of the agenda. Once again, no Wi-Fi access in the session breakout, so I'll publish this at the first available opportunity.

This is BC1693, Architecting DR Solutions with VMware SRM. The presenters are John Arrasjid and Will Crittenden; these are two solid guys that know DR and know SRM very well.

John starts the session with an overview of the influencing factors that affect a DR solution using SRM. Some of these factors may affect what you may or may not be able to do with SRM.

Clearly, there are different types of disasters. Some of these are true disasters---Hurricane Ike in Galveston, for example, or power outages---and there are "planned" failovers. Each of these needs to be accommodated in the design.

Some key questions to consider for DR design:

* What applications are mission critical?

* Is availability or performance more important?

* How much of my business capacity will run at the remote site and for how long with I be able to sustain that load?

* What distance is required to protect against goegraphic disasters?

* What technologies (hardware and software) will be needed?

* How often will you test the DR plan?

* What impact will DR plan testing have on the production site? What impact is acceptable to the business?

* What is the budget for this DR solution?

The three key network influencing factors are distance, bandwidth, and hop count. Throughput is good, but latency must also be considered. The type of replication, synchronous vs. asynchronous, that is being used is also important.

VLANs with SRM can be done in two ways: flat VLANs and disparate VLANs. With flat VLANs, no IP reconfiguration is required; with disparate VLANs, SRM can automate the process of reconfiguration IP addresses on VMs during the failover process.

Compliance guidelines that impact the business also need to be considered and incorporated into the design. Things like manual vs. automatic, SLA/RPO/RTO, failback requirements, security and access controls, and which technologies to use are all important. What about requirements to ensure that data is isolated to its own media?

What makes a DR solution successful? First, you need to understand what part of the business need to be protected. Understanding the applications and the dependencies (upstream and downstream) will help in this area. Ongoing testing of the DR plan is another key factor. The core virtualization itself is important---do you have the right version, is it correctly architected, are resources appropriately managed, etc. And, finally, operational readiness is important as well. Teams need to be trained on the different technologies and need to understand the workflows created within SRM.

When setting up SRM, two VirtualCenters and two SRM servers are required. Back-end SQL servers are necessary as well, in addition to authentication servers, and of course a supported data replication mechnaism. SRM should not be used to protect "shared services," like authentication (Active Directory), although these certainly can be virtual machines.

Inventory mapping is used to map networks/port groups at the Protected Site to the coresponding networks/port groups on the Recovery Site. The SRA (Storage Replication Adapter) handles the matching of LUNs between the Protected Site and the Recovery Site. Empty LUNs (LUNs without a VM) won't be properly recognized by SRM. Protection Groups form the basis of the Recovery Plan and are centered around LUNs. When a LUN is failed over, all VMs on that LUN must be failed over at the same time. This may require some re-organization of the VMs on the various LUNs to group VMs together for similar service levels/failover requirements.

In the Recovery Plan, high priority VMs are started sequentially, in the order defined in the plan; medium priority and low priority VMs are started in parallel. It's critical that business requirements and dependencies are understood here so that systems can be failed over and restarted in the correct order.

John now moved deeper into the design considerations, like server types, network configuration, DNs services, Active Directory services, VirtualCenter infrastructure (two VC servers, one at each site), and ESX hosts (needed at both sites). Of course, SRM servers are needed at both sites. Distributed Power Management (DPM) may also play a role here to help reduce power costs for VMware ESX hosts at the Recovery Site.

Will and John then proceeded to review some sample logical diagrams, sample recovery plan, sample workflow based on the sample recovery plan, and to discuss in more detail these various items.

Overall, the session is very good, but it is much more business-oriented and not technology-oriented. That may be due in large part to the nature of SRM; in order to be successful in building a DR solution, a strong business focus is required. If nothing else, it would be important for attendees of this session to at least understand that a successful SRM implementation involves much, much more than just installing and configuring SRM.
