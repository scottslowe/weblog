---
author: slowe
categories: Education
comments: true
date: 2009-11-27T10:03:52Z
slug: understanding-npiv-and-npv
tags:
- Cisco
- FibreChannel
- HP
- NPIV
- NPV
- Storage
- Networking
title: Understanding NPIV and NPV
url: /2009/11/27/understanding-npiv-and-npv/
wordpress_id: 1746
---

Two technologies that seem to have come to the fore recently are NPIV (N\_Port ID Virtualization) and NPV (N_Port Virtualization). Judging just by the names, you might think that these two technologies are the same thing. While they are related in some aspects and can be used in a complementary way, they are quite different. What I'd like to do in this post is help explain these two technologies, how they are different, and how they can be used. I hope to follow up in future posts with some hands-on examples of configuring these technologies on various types of equipment.

First, though, I need to cover some basics. This is unnecessary for those of you that are Fibre Channel experts, but for the rest of the world it might be useful:

* **N_Port:** An N_Port is an end node port on the Fibre Channel fabric. This could be an HBA (Host Bus Adapter) in a server or a target port on a storage array.

* **F_Port:** An F\_Port is a port on a Fibre Channel switch that is connected to an N\_Port. So, the port into which a server's HBA or a storage array's target port is connected is an F_Port.

* **E_Port:** An E\_Port is a port on a Fibre Channel switch that is connected to another Fibre Channel switch. The connection between two E\_Ports forms an _Inter-Switch Link (ISL)_.

There are other types of ports as well---NL\_Port, FL\_Port, G\_Port, TE\_Port---but for the purposes of this discussion these three will get us started. With these definitions in mind, I'll start by discussing N_Port ID Virtualization (NPIV).

## N_Port ID Virtualization (NPIV)

Normally, an N_Port would have a single N_Port_ID associated with it; this N_Port_ID is a 24-bit address assigned by the Fibre Channel switch during the FLOGI process. The N_Port_ID is not the same as the World Wide Port Name (WWPN), although there is typically a one-to-one relationship between WWPN and N_Port_ID. Thus, for any given physical N_Port, there would be exactly one WWPN and one N_Port_ID associated with it.

What NPIV does is allow a single physical N_Port to have multiple WWPNs, and therefore multiple N_Port_IDs, associated with it. After the normal FLOGI process, an NPIV-enabled physical N_Port can subsequently issue additional commands to register more WWPNs and receive more N_Port_IDs (one for each WWPN). The Fibre Channel switch must also support NPIV, as the F_Port on the other end of the link would "see" multiple WWPNs and multiple N_Port_IDs coming from the host and must know how to handle this behavior.

Once all the applicable WWPNs have been registered, each of these WWPNs can be used for SAN zoning or LUN presentation. There is no distinction between the physical WWPN and the virtual WWPNs; they all behave in exactly the same fashion and you can use them in exactly the same ways.

So why might this functionality be useful? Consider a virtualized environment, where you would like to be able to present a LUN via Fibre Channel to a specific virtual machine only:

* _Without NPIV_, it's not possible because the N_Port on the physical host would have only a single WWPN (and N_Port_ID). Any LUNs would have to be zoned and presented to this single WWPN. Because all VMs would be sharing the same WWPN on the one single physical N_Port, any LUNs zoned to this WWPN would be visible to all VMs on that host because all VMs are using the same physical N_Port, same WWPN, and same N_Port_ID.

* _With NPIV_, the physical N_Port can register additional WWPNs (and N_Port_IDs). Each VM can have its own WWPN. When you build SAN zones and present LUNs using the VM-specific WWPN, then the LUNs will only be visible to that VM and not to any other VMs.

Virtualization is not the only use case for NPIV, although it is certainly one of the easiest to understand.

&lt;aside&gt;As an aside, it's interesting to me that VMotion works and is supported with NPIV as long as the RDMs and all associated VMDKs are in the same datastore. Looking at how the physical N_Port has the additional WWPNs and N_Port_IDs associated with it, you'd think that VMotion wouldn't work. I wonder: does the HBA on the destination ESX/ESXi host have to "re-register" the WWPNs and N_Port_IDs on that physical N_Port as part of the VMotion process?&lt;/aside&gt;

Now that I've discussed NPIV, I'd like to turn the discussion to N_Port Virtualization (NPV).

## N_Port Virtualization

While NPIV is primarily a host-based solution, NPV is primarily a switch-based technology. It is designed to reduce switch management and overhead in larger SAN deployments. Consider that every Fibre Channel switch in a fabric needs a different domain ID, and that the total number of domain IDs in a fabric is limited. In some cases, this limit can be fairly low depending upon the devices attached to the fabric. The problem, though, is that you often need to add Fibre Channel switches in order to scale the size of your fabric. There is therefore an inherent conflict between trying to reduce the overall number of switches in order to keep the domain ID count low while also needing to add switches in order to have a sufficiently high port count. NPV is intended to help address this problem.

NPV introduces a new type of Fibre Channel port, the NP_Port. The NP_Port connects to an F_Port and acts as a proxy for other N_Ports on the NPV-enabled switch. Essentially, the NP_Port "looks" like an NPIV-enabled host to the F_Port on the other end. An NPV-enabled switch will register additional WWPNs (and receive additional N_Port_IDs) via NPIV on behalf of the N_Ports connected to it. The physical N_Ports don't have any knowledge this is occurring and don't need any support for it; it's all handled by the NPV-enabled switch.

Obviously, this means that the upstream Fibre Channel switch must support NPIV, since the NP_Port "looks" and "acts" like an NPIV-enabled host to the upstream F_Port. Additionally, because the NPV-enabled switch now looks like an end host, it no longer needs a domain ID to participate in the Fibre Channel fabric. Using NPV, you can add switches and ports to your fabric without adding domain IDs.

So why is this functionality useful? There is the immediate benefit of being able to scale your Fibre Channel fabric without having to add domain IDs, yes, but in what sorts of environments might this be particularly useful? Consider a blade server environment, like an HP c7000 chassis, where there are Fibre Channel switches in the back of the chassis. By using NPV on these switches, you can add them to your fabric without having to assign a domain ID to each and every one of them.

Here's another example. Consider an environment where you are mixing different types of Fibre Channel switches and are concerned about interoperability. As long as there is NPIV support, you can enable NPV on one set of switches. The NPV-enabled switches will then act like NPIV-enabled hosts, and you won't have to worry about connecting E_Ports and creating ISLs between different brands of Fibre Channel switches.

I hope you've found this explanation of NPIV and NPV helpful and accurate. In the future, I hope to follow up with some additional posts---including diagrams---that show how these can be used in action. Until then, feel free to post any questions, thoughts, or corrections in the comments below. Your feedback is always welcome!

_Disclosure: Some industry contacts at Cisco Systems provided me with information regarding NPV and its operation and behavior, but this post is neither sponsored nor endorsed by anyone._
