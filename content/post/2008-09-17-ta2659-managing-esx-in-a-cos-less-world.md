---
author: slowe
categories: Liveblog
comments: true
date: 2008-09-17T16:58:24Z
slug: ta2659-managing-esx-in-a-cos-less-world
tags:
- CLI
- ESX
- ESXi
- Hardware
- Virtualization
- VMware
- VMworld2008
title: 'TA2659: Managing ESX in a COS-less World'
url: /2008/09/17/ta2659-managing-esx-in-a-cos-less-world/
wordpress_id: 927
---

This is TA2659, Managing ESX in a COS-less World. The focus here is on tools that allow you to manage ESX without using the Service Console.

When I arrived at the session, the presenter was discussing VMware's goal to drive parity between the "actual" CLI present in the Service Console and the Remote CLI. There is a desire to ensure that any command that can be run on ESX or ESXi can also be run on the other.

Another tool to use in managing ESX without the Service Console is the VI Toolkit for PowerShell. I'm sure that most readers are already familiar with the VI Toolkit, so I won't go into any great detail there. There are about 120 cmdlets or so in the Toolkit today; another 50 or so are slated for release in the next version of the VI Toolkit. In addition, VI Toolkit management/compatibility is a core design facet---everything needs to be manageable via the VI Toolkit.

The VI Perl Toolkit is another scripting toolkit that can be used to manage ESX without relying upon the Service Console. This works on both Linux and Windows, where as the VI Toolkit only works on Windows. The `vicfg-*` tools are built on Perl.

Future directions include enhancements to the VI Perl Toolkit to expose functionality as Perl functions. A VI Java Toolkit will be available soon (within a month?). Other toolkits may become available depending upon market demand and direction.

From the perspective of server health monitoring, CIM SMASH is the direction in which VMware is moving. CIM (Common Information Model) is both a protocol and a data model representing functionality. SMASH (Systems Management Architecture for Server Hardware) is the data model for hardware health monitoring and management. An example of a tool that works with CIM SMASH is WinRM, which ships on Windows Vista and presumably Windows Server 2008. CIM SMASH profiles simply define the sensors and the values that should be retrieved for various hardware elements.

A fair amount of CIM SMASH functionality was exposed in ESX 3.5 Update 2. This is done per-host; in the future, VirtualCenter will aggregate that for multiple hosts.

An attendee asked a question about varying degrees of hardware monitoring being exposed in the initial release of ESXi and CIM support within both ESXi and the underlying hardware. The response is that this functionality is driven by both SIM providers, which are written by either VMware or the hardware vendor; in most cases, it's VMware ESXi that needs to beef up the SIM providers.

In the future, the vision of VMware is to abstract the configuration into a configuration file. This configuration template could then be applied to multiple hosts, and the configuration can be assessed regularly to verify configuration, compliance, etc. The information required to do all this is already being handled by the web services API in VirtualCenter, and the tools necessary to perform the configuration are already present in the API (the `esxcfg-*` and `vicfg-*` commands already leverage the API to do these very tasks). Combining these two things would allow us to create a configuration template that can be applied to a host while still allowing for customization (like VM customization during cloning).

The subject of deployment is a key issue when we think about losing the Service Console. One approach to handling these issues is deploying physical machines; another would be to deploy virtual machines to handle these tasks. Partners could wrap up the agents that would typically be deployed in the Service Console as a virtual appliance, but then users could end up with numerous virtual appliances. What if VMware were to provide a virtual infrastructure management appliance? That's what VIMA (Virtual Infrastructure Management Assistant) is.

VIMA is a virtual appliance packaged as OVF and is distributed, maintained, and supported by VMware. This is downloaded and installed by the customer according to their management procedures. This will be a well-known deployment environment that partners can rely upon being present. This will be a 64-bit Linux distribution with VMware Tools, VI Perl Toolkit, the Remote CLI (now known as the VI CLI), and a JRE already present. VIMA can be patched for updates, and it allows you to manage one or more VMware ESX hosts directly or through VirtualCenter. VIMA can enable agents to authenticate themselves, and VIMA will rotate its passwords on the hosts. Additionally, sample code and documentation will be available for programming applications to work in VIMA.

In "classic" ESX, management agents and hardware agents ran in the Service Console; with VIMA, updated management agents will talk through the VI API and hardware agents will talk through CIM SMASH. An example of this is the APC PowerChute Network Shutdown (PCNS), which is being rewritten to use the VI API and will run in VIMA.

Anyone interested in VIMA can e-mail vima_request@vmware.com and request access to pre-GA versions of VIMA. VIMA is expected for general release in the fourth quarter of this year. All VIMA releases will work with both ESX and ESXi (again, pointing to the desire to keep parity between these two products).

Future versions of VIMA may add Active Directory support; authentication through vCenter Servers; improved automation, configuration, and updates; UI integration in the VI Client; additional VMware components pre-installed. Finally, VMware Studio will be used to build future versions of VIMA.

At this point, the presentation ended and the floor was opened up for a question and answer session.
