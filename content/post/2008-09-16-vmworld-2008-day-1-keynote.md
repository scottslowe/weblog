---
author: slowe
categories: Liveblog
comments: true
date: 2008-09-16T12:10:23Z
slug: vmworld-2008-day-1-keynote
tags:
- Virtualization
- VMware
- VMworld2008
title: VMworld 2008 Day 1 Keynote
url: /2008/09/16/vmworld-2008-day-1-keynote/
wordpress_id: 905
---

I arrived late and couldn't find the bloggers' table, so I had to find a table. Unfortunately, I couldn't get an Internet connection, so publishing this keynote will have to wait until after the keynote is over. Sorry guys!

Paul Maritz started out the keynote with a history of what has brought VMware to where it is today. He recognized the contributions of Diane Greene and Mendel Rosenblum, two of VMware's founders that are no longer with VMware, in creating VMware's technologies and products. He proceeded to list all the various movements and technology changes that have shaped VMware and the industry as a whole, to bring us to a point where we are today. Paul indicates that all these forces come together to bring the industry to a point where we are no longer focused in a device-centric way, but rather in a people-centric and data-centric view. This brings VMware to three key themes:

* Virtual Datacenter OS
* vCloud initiative
* vClient initiative

The driving forces behind these three themes are being mandated to use compute resources and applications more efficiently and more flexibly, to create "internal clouds" or act as a hosting provider to internal customers. In addition, organizations need to scale these compute resources outside the firewall, so VMware needs to federate the internal cloud with these external clouds. Finally, the view of IT needs to be people-centric and information-centric. This is the piece that will solve the "desktop dilemma."

## Virtual Datacenter OS

This first initiative by VMware is intended to address the need for organizations to use compute resources and applications more flexibly and more efficiently. In order to achieve this goal as fully and as completely as organizations require, a new level of abstraction is required. Paul Maritz believes that this will occur across the industry, not just in VMware. VMware recognizes the need to grow, formalize, and harden VMware Infrastructure into the Virtual Datacenter Operating System (VDC-OS).

VDC-OS will provide vServices, both on the infrastructure side as well as on the application side. Infrastructure vServices include vCompute, vNetwork, vStorage. Application vServices include such things as availability, security, and scalability. In the future, more vServices can be introduced to extend the VDC-OS.

As an example of a way in which the vCompute infrastructure vService can be extended, Maritz points to the formal introduction by Intel of a new chipset that officially supports FlexMigration. FlexMigration enables Enhanced VMotion, which in turn enables heterogeneous VMotion between disparate CPU revisions.

As another example of the way in which the vNetwork infrastructure vService can be extended, Paul makes mention of the Cisco vSwitch that is being announced this week.

vStorage can be extended as well; Paul points to Site Recovery Manager's SRA architecture as the way in which VMware allows storage vendors to exploit their array-specific functionality within VMware environments.

All of these examples just provide further proof that the best way to view VDC-OS is as a framework into which third party ISVs, and even VMware, can plug new vServices to extend or modify the application or infrastructure capabilities of the overall solution.

Moving into a discussion of application vServices, Paul makes mention of vApps, the next evolution of virtual appliances, which embraces OVF and allows multiple VMs to be included in the vApp definition. Also, the vApp definition can include more metadata, like SLA or DR characteristics.

Management services are also being extended along with the VDC-OS. vCenter is the next evolution of VirtualCenter, which brings along management vServices like AppSpeed (formerly B-Hive Conductor), ConfigControl, Orchestrator (formerly Dunes), and CapacityIQ. vCenter will also integrate and federate with external solutions from partners like BMC Software, HP, CA, and IBM/Tivoli. This is yet another example of how vServices can be extended to include additional functionality and capabilities.

In summary, the VDC-OS enables organizations to address the key business drivers:

* Infrastructure efficiency

* Application agility

* Business-driven IT

With that, Paul transitions into a discussion of federation with the cloud.

## vCloud

Federation with external clouds is being driven by the same business factors that are driving VDC-OS. Unfortunately, external clouds are typically not compatible with enterprise IT, they require application modification, and they are proprietary and not interoperable. VMware believes that applying the same principles within the datacenter to the cloud will address these obstacles. This will be enabled by the use of vApps---which are defined in a standard, interoperable way---and cloud vServices. The cloud vServices enable external clouds to speak and communicate with internal enterprise clouds. Paul then moves into a live demo of some of these technologies.

The first demo was a vCloud demo, which allows a local cloud to interoperate with an off-premise cloud. This policy will be driven by user experience, i.e., the SLA must be less than 4 seconds. The policy also indicates what to do if the SLA isn't being met, like resuming a suspended VM to accommodate spikes in demand. The demo involved the future product AppSpeed. In the demo, artificial demand was placed on an application and as the SLA was breached, AppSpeed automatically provisioned capacity out of an external cloud to bring the response times back down to within SLA limits.

## Desktop Dilemma

IT needs to bring down costs on the desktop side, but there are lots of questions. Thin clients, or traditional PCs? Which client operating system? How do we handle laptops? This is an example of IT thinking in a device-centric way, instead of thinking in an information-centric or people-centric way.

Focusing on "desktops that follow the user" leads to the renaming of VDI to VMware View. This is an extension of the traditional VDI environment that not only allows VMs to be accessed in the data center, but that also allows a VM to be sucked down to a traditional fat client, using client virtualization, and executed locally but synchronized with the data center. VMware is a leader in client virtualization via VMware Workstation and VMware Fusion, and this leadership allows them to capitalize on technology leadership in two areas to deliver desktops that will follow the user. Naturally, this can be extended moving forward to include things like iPhones....er, other form factors.

To show off this technology, Paul moves into a demo. First, we see a "traditional" VDI environment, where a user logs into an HP thin client that connects via VDM to a VM hosted in the data center. But what about disconnected devices? Paul and Phil (his helper) boot up a Lenovo laptop with a USB key to show how a disconnected device can also access the same desktop with a satisfactory rich user experience. To help address that rich user experience, Paul and Phil show off a Indiana Jones and extensive 3-D graphics running in a virtualized environment. This helps to address the concerns over a rich user experience.

The evolution from VDI to VMware View enables server-based management, a rich PC user experience, and universal clients. As part of this evolution, VMware is co-developing a new protocol with Teradici and supporting HP's RGS protocol, continuing to enhance 3-D support within client virtualization, and extending these capabilities to new platforms and new form factors.

With that, Paul summarized his presentation by bringing it back again to the internal cloud (VDC-OS), scale by connecting with the external clouds (vCloud), and solving the desktop dilemma (vClient).

The real question is: will it be moist and chewy? (This is from the Gates/Seinfeld ad series currently running.)
