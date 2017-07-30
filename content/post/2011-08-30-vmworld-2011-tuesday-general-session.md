---
author: slowe
categories: Liveblog
comments: true
date: 2011-08-30T11:25:35Z
slug: vmworld-2011-tuesday-general-session
tags:
- Virtualization
- VMware
- VMworld2011
title: VMworld 2011 Tuesday General Session
url: /2011/08/30/vmworld-2011-tuesday-general-session/
wordpress_id: 2385
---

This is a session blog for the Day 2 general session at VMworld 2011 in Las Vegas. Today everyone here at VMworld is looking forward to the in-depth technical discussions that typically follow after Paul Maritz' more high-level presentation the previous day. VMware CTO, Steve Herrod, should be one of the primary if not the primary speaker today.

The session begins with some videos from various VMware employees discussing their products: vCenter Operations, vFabric and vFabric Data Director, vSphere. The ending quote: "It should just work and work well."

Dr. Stephen Herrod, CTO of VMware, then takes the stage. Steve starts his discussion with describing the shift from thinking of individual servers and individual PCs to thinking of services (the cloud) and people (the users). Thinking about the users means thinking about all the devices that people want to use, universal access to information, and high expectations. He next goes to a video that describes how VMware is thinking about the "post-PC" era. The video is decidedly marketing-focused, but Steve follows up the discussion with a deeper look at the actual mechanics and tools used to help achieve that vision.

There are three key tasks that Steve thinks must be accomplished in order to achieve the "post-PC" era: simplify, manage, and connect.

To simplify, VMware looks to tools like VMware View and ThinApp, to make Windows desktop and Windows applications easier to use and manage. The next step is an application catalog, and data management, to keep users' data accessible regardless of where the user is or what device the user is using. This is managed/driven by a unified service broker, and the unified service broker then provides connectivity to all the various devices that a user might use (iPad, PC, mobile phone, etc.). Steve will take a look at this architecture and the tools from the perspective of IT as well as the perspective of the end user.

The first demonstration is VMware View 5 provisioning a pool of virtual desktops using linked clones. Nothing really new or exciting here, except perhaps a look at the View 5 user interface.

The next demo is something called Project ThinApp Factory, a tool to automate the process of extracting applications out of Windows using ThinApp, and Horizon, an application portal for provisioning applications to people. The demo shows the web interface for Project ThinApp Factory being placed into Horizon's application catalog, showing both SaaS and traditional Windows-based applications in the same application catalog. Horizon will also include mobile applications and broker access to non-VMware reesources, such as Citrix XenApp.

Steve now moves on to something called Project Octopus. Project Octopus seeks to provide corporate IT with a way of running their own Dropbox-style sevice for data management services. The demo shows a web interface for Octopus and how IT can allow a user to access this service and how IT can provide simple but reasonably secure sharing of data with external entities. Project Octopus can be delivered via both private cloud and public cloud models.

So far, Steve has shown us---from an IT administrator's perspective---provisioning desktops, extracting applications, connecting users with applications via Horizon and connecting users with data via Project Octopus. At this point, Vittorio (Product Manager for EUC at VMware) comes on stage to show us the end-user's perspective. He walks through using the Horizon interface to access applications, see documents, and activate his mobile phone. Vittorio demonstrates Mobile Virtualization Platform (MVP) on an LG Android phone, showing a personal phone instance and a virtual phone instance as well.

Steve takes the stage again to break down Vittorio's demo. He points out the applications and technologies that were shown:

* VMware View 5 with virtual profiles

* Horizon

* Horizon Mobile (the new name for MVP)

* Project Octopus

Steve announces a new partnership with Samsung to provide Horizon Mobile services on their Android-powered phones. This is in addition to the partnership that VMware already has with LG. Additional partnerships are in the works.

Vittorio now takes the stage again, this time as a mobile user out of the office. In this demonstration, he's using an iPad and SocialCast, and then starts using Project Octopus to access files on his iPad. He attempts to edit an Excel file on his iPad, which opens Mobile Safari and then runs a version of Excel inside Mobile Safari using something called AppBlast. Vittorio next does a video call to a call center, where the agent, who uses a couple of different applications to assist Vittorio. (Presumably the call center agent is using a virtual desktop on VMware View.)

So now Steve takes the stage again to break down Vittorio's demo:

* Project Octopus to access files from his iPad

* Project AppBlast, which converts traditional applications being converted into HTML5 to run on any HTML5-compliant client (Windows, Linux, or Mac apps)

* VMware View 5 was running in the call center, showing off unified communications, Aero user interface, high-speed graphics with PCoIP over a wide area network (WAN)

Steve points out that Vittorio "accidentally" forgot his mobile device; this is to illustrate that IT could wipe/delete the corporate IT managed phone instance on that device.

Steve wraps up his end-user computing discussion by re-iterating the role of Horizon as the central unified broker, bringing together technologies like VMware View, Project ThinApp Factory, Project Octopus, Horizon Mobile, and Project AppBlast. This sets the stage for him to shift the discussion to the infrastructure that supports these efforts.

Steve shows off a preview of the next version of the vSphere iPad Client, where vMotion is now enabled. I'm not so sure about the user interface---it seems a bit gimmicky---but I'm glad to see VMware continuing to push into new client markets and provide new functionality.

The next demonstration is a demo of VMware Go, a hosted virtualization service designed for the SMB market, and a demo of the vSphere Storage Appliance, a shared storage solution for the SMB market.

Steve shifts to a discussion of Auto Deploy. Auto Deploy is a PXE boot solution for ESXi hosts, and many bloggers have already discussed it in some detail. (There will also be full coverage of Auto Deploy in my upcoming vSphere 5 book.)

The next discussion is about VM scalability: 32 vCPUs, 1 TB of RAM, and up to 1,000,000 IOPs per vSphere host.

Steve shifts the discussion to more of a focus on policy-based administration, i.e., "VM contracts". This leads Steve into a discussion of performance guarantees and storage, and that brings up three new features in vSphere 5:

* Logical pooling of storage (datastore clusters)

* VM placement (VM storage profiles, VASA, Storage DRS)

* Load/usage balancing (Storage DRS)

To address more short-term storage performance concerns, vSphere offers Storage I/O Control (present in vSphere 4.1 as well). This addresses more short-term concerns while Storage DRS can address longer-term balancing. There's also Network I/O Control, which provides similar controls at the network layer.

The mention of networking lets Steve focus a bit more on networking. Steve discusses the "identifier = location" problem, and the solution is VXLAN, which encapsulates L2 packets into L3 packets. This is intended to address the "identifier = location" problem, but is it standards-based? Why create a new VXLAN standard instead of leveraging existing solutions? VMware says that VXLAN has been created in collaboration with Cisco, Arista, Broadcom, Emulex, and Intel. The specification has been submitted to the IETF, but again I would question why they felt the need to create a new standard instead of leveraging an existing solution.

Next on stage is a discussion of Site Recovery Manager 5, which provides a DR workflow solution. Naturally, Steve brings up vSphere Replication, which provides host-based replication of VMs from one site to another site. SRM 5 also provides automated failback, and some vCloud partners are providing "DR to the cloud" solutions using SRM 5.

vShield 5 is the next topic in Steve's talk. There are several different vShield offerings:

* vShield Endpoint

* vShield App

* vShield Edge

vShield App incorporates DLP technologies (taken from RSA), which provides some additional functionality to ensure sensitive data is appropriately protected.

Steve switches now to management. Performance, availability, and security are great, but management is important and necessary, and Steve indicates that VMware has a large number of engineers currently focused on management. Steve shows off some demonstrations of new versions of various VMware management applications, like vCenter Operations.

At this point the keynote is wrapping up.
