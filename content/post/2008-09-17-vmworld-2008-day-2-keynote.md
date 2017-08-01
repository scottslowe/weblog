---
author: slowe
categories: Liveblog
comments: true
date: 2008-09-17T10:42:43Z
slug: vmworld-2008-day-2-keynote
tags:
- Virtualization
- VMware
- VMworld2008
title: VMworld 2008 Day 2 Keynote
url: /2008/09/17/vmworld-2008-day-2-keynote/
wordpress_id: 914
---

Today promises to be an easier keynote, since I actually managed to find the bloggers' tables (they actually exist) and get good wireless connectivity. In addition, today's keynote should be more technical and feature-focused than Paul's keynote yesterday. Refresh this page often, as I'll be updating this post throughout the keynote.

The keynote starts off with a series of video testimonials about VMware's drive forward in technology, about how applications are served better, about how availability is improved, etc.

Afterwards, Dr. Stephen Herrod, CTO and Senior VP of R&D; for VMware takes the stage. Stephen indicates that he's going to follow up with technical details on VDC-OS, vCloud, and vClient. He describes VDC-OS as a set of roadmap initiatives that will drive forward technologies to achieve the goals that Paul described yesterday in his keynote.

Focusing first on the infrastructure layer, he indicates that the goal is to aggregate resources and allocate them to workloads efficiently and appropriately. In the vCompute area of the infrastructure layer, Stephen first indicates that one area is beefing up individual VMs. The goal for VM scalability in the vCompute area is to hit 8 vCPUs, 256GB of RAM, 40 Gbps of network throughput, and 200,000 IOPS.

However, extending the power of individual VMs isn't enough. VMware needs to do even more in terms of aggregating vCompute resources, enabling clusters containing up to 64 hosts with up to 4,096 processor cores, etc. All of this will be load balanced using VMware DRS.

Dr. Herrod next discusses DPM (Distributed Power Management) to reduce power consumption. He did not indicate when DPM will move out of experimental support.

Moving on to the next infrastructure area, vStorage, the idea here is to fit VMware Infrastructure and partner storage together in a way to leverage partner-specific features. vStorage encompasses the intersection of VMware-created technologies, like VMFS and Storage VMotion, and partner-enabled technologies. New features that are being announced include thin provisioning and linked clones. These types of functionality often exist in partner storage arrays as well, and can sometimes be performed more quickly or more efficiently.

First up with more detail is vStorage Thin Provisioning. It appears that the key focus is on alerting and reporting frameworks to protect customers from overallocating. Dr. Herrod refers to the vStorage APIs; I think this is the first time I've heard it referenced that way, but it does make sense. Still no word on the potential interaction between thin provisioning and VMware FT.

vNetwork is up next as Stephen continues working through the infrastructure vServices. The focus here is much like with vStorage, in that VMware wants to leverage partner technologies and functionality. The discussion starts with a look at today's networking functionality (per-host vSwitches), then moves into the vNetwork Distributed Switch. (I wish they'd make up their mind what this will be called, as I'm seeing several different names.) He brings out the first major discussion of Network VMotion, which will bring the ability to move a VM's network state between hosts on the Distributed Switch.

Yesterday Cisco announced the Nexus 1000v yesterday, and Dr. Herrod points to this as an example of how VMware and Cisco worked together to leverage open APIs in VMware's products to extend the functionality of the solution.

At this point, Dr. Herrod wraps up his discussion of infrastructure vServices into a discussion of application vServices, and indicates that these functions and features are intended to be provided in an application-neutral way that will work with applications today and tomorrow, unmodified.

Stephen revisits the idea of the vApp, confirming that it is based on OVF. vApps may wrap together multi-tier applications, and the metadata embedded in the vApp can be applied to all the tiers in the multi-tier application. The most commonly mentioned metadata is SLA.

Availability is a key focus out of the application vServices. Dr. Herrod walks through the various technologies that VMware has created over time to help improve availability, both from a planned and an unplanned perspective. This includes stuff like NIC teaming, storage multipathing, VMotion, Storage VMotion, VMware HA, and Site Recovery Manager. The new feature that Stephen announces is VMware Fault Tolerance (FT), which is intended to protect against unplanned server failure. He distinguishes VMware HA from VMware FT by indicating that VMware HA is stateless and reboots the workloads in the event of a failure. I discussed VMware FT in-depth in yesterday's [session notes][1], so I won't bother to go into a lot of detail here. Next we'll see a demonstration of VMware FT in action.

Mark Vaughn, of First American Corporation, takes the stage to discuss his experience with VMware FT. The demo shows an application being protected, first by showing how to enable VMware FT. Mark demonstrates turning FT on by a simple right-click operation within VirtualCenter. At this point, the secondary VM is being created using a special VMotion; all this is being represented in VirtualCenter's Tasks pane during the demo. Once the secondary is created, vLockstep will keep the two VMs in synchronization. Mark points out the new VMware FT pane, which shows the status and the host running the secondary VM. Now that VMware FT is running, Stephen will "accidentally" power off one of the servers. As shown on the stage, the slot machine VM continues running, and VirtualCenter automatically spawns a new secondary VM and re-enables VMware FT protection for the VM.

Now we move on to security application vServices, and Dr. Herrod takes us back to the vApp and indicates that security policies may be embedded in the vApp's metadata. VMsafe is the underlying building block that allows security vServices to be created. The functionality enabled by VMsafe and security vServices enables potentially higher levels of security, since the security VMs that are running using these APIs can see both network traffic and process activity, and can correlate that data to provide better protection.

The distributed vSwitch also enables greater security because network state is maintained after a VMotion operation (what is referred to as Network VMotion). And, of course, partners will use the VMsafe APIs (why not call them vSecurity APIs?) to build a wide variety of solutions.

That wraps up the discussion of application vServices, and now he turns to management vServices. That means a discussion of vCenter. He discusses ConfigControl, vCenter Orchestrator, CapacityIQ, and Chargeback. These solutions extend the functionality of vCenter, and all of these APIs that partners can use to build their own solutions.

vCenter AppSpeed is another application management vService that VMware will continue to develop. This is built on the B-Hive acquisition. It works by discovering workloads and applications based on network traffic, the monitor them, and remediate application performance problems when necessary. Dr. Herrod mentions remediation, but I'm skeptical because not all applications can be scaled in the way in which they demonstrate. Some applications can scale (web-based front-ends, for example), but many cannot. AppSpeed won't really be able to do much in those cases, in my mind.

Asaf Wexler, founder of B-Hive, now takes the stage for a demo. They're using SugarCRM, a web-based application. Asaf shows how AppSpeed dynamically discovers the various components of the SugarCRM application as he works within the application. AppSpeed is even discovering and mapping database tables and specific queries as he uses the application. Asaf next moves on to demonstrating the UI for monitoring the application within VirtualCenter, and then discusses how remediation is handled. Through the demonstration, Asaf shows how it's not the application, not the resources allocated to the application VM, but instead the database VM. Utilizing hot-add technology that VMware is slated to include in future versions, additional resources could be applied to database VM to help resolve the issue.

Dr. Herrod points out that these features will be exposed via APIs for partners to leverage as well.

VirtualCenter Server is being ported over to Linux and being packaged and delivered as a virtual appliance. The crowd was very excited about that (lots of applause). In addition, VMware is working on multi-platform vCenter client, including Linux, Mac OS X, and other form factors. He jokes about using the iPhone's accelerometer to trigger a DR failover.

Stephen next moves on to discussing the cloud. He does recognize that the term "cloud" is over-used and over-hyped. He describes some basics that define the cloud, and ties that to what VMware is doing with VDC-OS to address the challenges of creating a "cloud". Those challenges include application compatibility, lack of standardization, complexity and switching costs, a the need for new flexible, efficient infrastructure. Stephen seems to believe that OVF will help remove some of these standardization issues. Multi-tenancy is another key issue. All of these issues are being addressed in VDC-OS and the vCloud initiative.

vCloud will contain various APIs that address tasks like image management, user accounts, consistent chargeback mechanisms, and mobility. Mobility isn't necessarily about "Cloud VMotion" but also about moving between clouds, internal or external.

Stephen again revisits the vApp concept and OVF. vApp and OVF are considered to be an significant enabling technology in providing cloud federation.

Finally, he switched to the vClient initiative. The key thing here is switching the view from hardware to users and information. A user's "desktop" is really nothing more than a collection of applications and information. This view also needs to encompasses multiple access points, and all of this needs to be managed and secured. The vClient initiative is less about new technologies and more about evolving and extending VMware's 10 year history in client virtualization in three key areas: user experience, client virtualization, and centralized management.

With regards to user experience, the goal is to drive the user experience to the best possible level based on connectivity. VMware has been really driving user experience in the client virtualization area, where features like Unity and 3-D graphics. In the client virtualization area, VMware also wants to drive a thin hypervisor to provide bare metal virtualization, easy provisioning, and rich user experience. But this all ties back to centralized management, where we can control provisioning, image updating, and policy enforcement.

Next Jerry Chen of VMware takes the stage to discuss VMware View and the linked clones (vStorage Linked Clones). The first task in the demo is to create 25 desktops very quickly. This will be done very quickly and with tremendous storage savings.

The next task in the demo is to modify the image to include Google Chrome, as an example of updating images after they have been deployed. Jerry shows off his personal "desktop" running on the bare metal client hypervisor. Using VMware ThinApp, Jerry deploys Google Chrome, then recomposes the linked clones to include the changes he just made. His laptop gets a notification, he restarts and Google Chrome appears in his linked desktop image.

The third and final task for Jerry's demo is to show off policy enforcement. The example is revoking access to a distributed VMs. Using View Manager, Jerry edits the policy and denies access to the desktop. A warning pops up and the VM is shut down. Access to the VM has been revoked. Note that in all these demos network connectivity to VMware View Manager was assumed; it would be interesting to discuss how these features operate in a disconnected mode.

Dr. Herrod wraps up the keynote with a summary of the three major initiatives: VDC-OS, vCloud, and vClient. At this point the keynote wraps up.

[1]: {{< relref "2008-09-16-bc2621-fault-tolerant-vms-in-vi-operations-and-best-practices.md" >}}
