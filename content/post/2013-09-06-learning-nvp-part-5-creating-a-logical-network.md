---
author: slowe
categories: Tutorial
comments: true
date: 2013-09-06T09:00:00Z
slug: learning-nvp-part-5-creating-a-logical-network
tags:
- Networking
- Nicira
- NVP
- OVS
- Virtualization
title: 'Learning NVP, Part 5: Creating a Logical Network'
url: /2013/09/06/learning-nvp-part-5-creating-a-logical-network/
wordpress_id: 3272
---

I'm back with more NVP goodness; this time, I'll be walking you through the process of creating a logical network and attaching VMs to that logical network. This work builds on the stuff that has come before it in this series:

* In [part 1][1], I introduced you to the high-level architecture of NVP.

* In [part 2][2], I walked you through setting up a cluster of NVP controllers.

* In [part 3][3], I showed you how to install and configure NVP Manager.

* In [part 4][4], I discussed how to add hypervisors (KVM hosts, in this case) to your NVP environment.

Just a quick reminder in case you've forgotten: although VMware recently [introduced VMware NSX at VMworld 2013](http://blogs.vmware.com/networkvirtualization/2013/08/vmware-nsx.html), the architecture of NSX when used in a multi-hypervisor environment is _very_ similar to what you can see today in NVP. (In pure vSphere environments, the NSX architecture is a bit different.) As a result, time spent with NVP now will pay off later when NSX becomes available. Might as well be a bit proactive, right?

At the end of part 4, I mentioned that I was going to revisit a few concepts before proceeding to the logical network piece, but after deeper consideration I've decided to proceed with creating a logical network. I still believe there will be a time when I need to stop and revisit some concepts, but it didn't feel right just yet. Soon, I think.

Before I get into the details on how to create a logical network and attach VMs, I want to first talk about my assumptions regarding your environment.

## Assumptions

This walk-through assumes that you have an NVP controller cluster up and running, an instance of NVP Manager connected to that cluster, at least 2 hypervisors installed and added to NVP, and at least 1 VM running on each hypervisor. I further assume that your environment is using KVM and libvirt.

Pursuant to these assumptions, my environment is running KVM on Ubuntu 12.04.2, with libvirt 1.0.2 installed from the [Ubuntu Cloud Archive](https://wiki.ubuntu.com/ServerTeam/CloudArchive). I have the NVP controller cluster up and running, and an instance of NVP Manager connected to that cluster. I also have an NVP Gateway and an NVP Service Node, two additional components that I haven't yet discussed. I'll cover them in a near-future post.

Additionally, to make it easier for myself, I've created a libvirt network for the Open vSwitch (OVS) integration bridge, as outlined [here][5] (and an update [here][6]). This allows me to simply point `virsh` at the libvirt network, and the guest domain will attach itself to the integration bridge.

## Revisiting Transport Zones

I showed you how to create a transport zone in [part 4][4]; it was necessary to have a transport zone present in order to add a hypervisor to NVP. But what is a transport zone? I didn't explain it there, so let me do that now.

NVP uses the idea of transport zones to provide connectivity models based on the topology of the underlying network. For example, you might have hypervisors that connect to one network for management traffic, but use an entirely different network for VM traffic. The combination of a transport zone plus the transport connectors tells NVP how to form tunnels between hypervisors for the purposes of providing logical connectivity.

For example, consider this graphic:

![Transport zone illustration](/public/img/nvp-transport-zones.png)

The transport zones (TZ-01 and TZ-02) help NVP understand which interfaces on the hypervisors can communicate with which other interfaces on other hypervisors for the purposes of establishing overlay tunnels. These separate transport zones could be different trust zones, or just reflect the realities of connectivity via the underlying physical network.

Now that I've explained transport zones in a bit more detail, hopefully their role in adding hypervisors makes a bit more sense now. You'll also need a transport zone already created in order to create a logical switch, which is what I'll show you next.

## Creating the Logical Switch

Before I get started taking you through this process, I'd like to point out that this process is going to _seem_ laborious. When you're operating outside of a CMP such as CloudStack or OpenStack, using NVP will require you to do things manually that you might not have expected. So, keep in mind that NVP was designed to be integrated into a CMP, and what you're seeing here is what it looks like without a CMP. Cool?

The first step is creating the logical switch. To do that, you'll log into NVP Manager, which will dump you (by default) into the Dashboard. From there, in the Summary of Logical Components section, you'll click the Add button to add a switch. To create a logical switch, there are four sections in the NVP Manager UI where you'll need to supply various pieces of information:

1. First, you'll need to provide a display name for the new logical switch. Optionally, you can also specify any tags you'd like to assign to the new logical switch.

2. Next, you'll need to decide whether to use port isolation (sort of like PVLANs; I'll come back to these later) and how you want to handle packet replication (for BUM traffic). For now, leave port isolation unchecked and (since I haven't shown you how to set up a service node) leave packet replication set to Source Nodes.

3. Third, you'll need to select the transport zone to which this logical switch should be bound. As I described earlier, transport zones (along with connectors) help define connectivity between various NVP components.

4. Finally, you'll select the logical router, if any, to which this switch should be connected. We won't be using a logical router here, so just leave that blank.

Once the logical switch is created, the next step is to add logical switch ports.

## Adding Logical Switch Ports

Naturally, in order to connect to a logical switch, you need logical switch ports. You'll add a logical switch port for each VM that needs to be connected to the logical switch.

To add a logical switch port, you'll just click the Add button on the line for Switch Ports in the Summary of Logical Components section of the NVP Manager Dashboard. To create a logical switch port, you'll need to provide the following information:

1. You'll need to select the logical switch to which this port will be added. The drop-down list will show all the logical switches; once one is selected that switch's UUID will automatically populate.

2. The switch port needs a display name, and (optionally) one or more tags.

3. In the Properties section, you can select a port number (leave blank for the next port), whether the port is administratively enabled, and whether or not there is a logical queue you'd like to assign (queues are used for QoS; leave it blank for no queue/no QoS).

4. If you want to mirror traffic from one port to another, the Mirror Ports section is where you'll configure that. Otherwise, just leave it all blank.

5. The Attachment section is where you "plug" something into this logical switch port. I'll come back to this---for now, just leave it blank.

6. Under Port Security you can specify what address pairs are allowed to communicate with this port.

7. Finally, under Security Profiles, you can attach an existing security profile to this logical port. Security profiles allow you to create ingress/egress access-control lists (ACLs) that are applied to logical switch ports.

In many cases, all you'll need is the logical switch name, the display name for this logical switch port, and the attachment information. Speaking of attachment information, let's take a closer look at attachments.

## Editing Logical Switch Port Attachment

As I mentioned earlier, the attachment configuration is what "plugs" something into the logical switch. NVP logical switch ports support 6 different types of attachment:

* None is exactly that---nothing. No attachment means an empty logical port.

* VIF is used for connecting VMs to the logical switch.

* Extended Network Bridge is a deprecated option for an older method of bridging logical and physical space. This has been replaced by L2 Gateway (below) and should not be used. (It will likely be removed in future NVP releases.)

* Multi-Domain Interconnect (MDI) is used in specific configurations where you are federating multiple NVP domains.

* L2 Gateway is used for connecting an L2 gateway service to the logical switch (this allows you to bring physical network space into logical network space). This is one I'll discuss later when I talk about L2 gateways.

* Patch is used to connect a logical switch to a logical router. I'll discuss this in greater detail when I get around to talking about logical routing.

For now, I'm just going to focus on attaching VMs to the logical switch port, so you'll only need to worry about the VIF attachment type. However, before we can attach a VM to the logical switch, you'll first need a VM powered on and attached to the integration bridge. (Hint: If you're using KVM, use `virsh start <VM name>` to start the VM. Or just read [this][7].)

Once you have a VM powered on, you'll need to be sure you know the specific OVS port on that hypervisor to which the VM is attached. To do that, you would use `ovs-vsctl show` to get a list of the VM ports (typically designated as "vnet0", "vnet1", etc.), and then use `ovs-vsctl list port vnetX` to get specific details about that port. Here's the output you might get from that command:

In particular, note the `external_ids` row, where it stores the MAC address of the attached VM. You can use this to ensure you know which VM is mapped to which OVS port.

Once you have the mapping information, you can go back to NVP Manager, select Network Components > Logical Switch Ports from the menu, and then highlight the empty logical switch port you'd like to edit. There is a gear icon at the far right of the row; click that and select Edit. Then click "4. Attachment" to edit the attachment type for that particular logical switch port. From there, it's pretty straightforward:

1. Select "VIF" from the Attachment Type drop-down.

2. Select your specific hypervisor (must already be attached to NVP per [part 4][4]) from the Hypervisor drop-down.

3. Select the OVS port (which you verified just a moment ago) using the VIF drop-down.

Click Save, and that's it---your VM is now attached to an NVP logical network! A single VM attached to a logical network all by itself is no fun, so repeat this process (start up VM if not already running, verify OVS port, create logical switch port [if needed], edit attachment) to attach a few other VMs to the same logical network. Just for fun, be sure that at least one of the other VMs is on a different hypervisor---this will ensure that you have an overlay tunnel created between the hypervisors. That's something I'll be discussing in a near-future post (possibly part 6, maybe part 7).

Once your VMs are attached to the logical network, assign IP addresses to them (there's no DCHP in your logical network, unless you installed a DHCP server on one of your VMs) and test connectivity. If everything went as expected, you should be able to ping VMs, SSH from one to another, etc., all within the confines of the new NVP logical network you just created.

There's so much more to show you yet, but I'll wrap this up here---this post is already _way_ too long. Feel free to post any questions, corrections, or clarifications in the comments below. Courteous comments (with vendor disclosure, where applicable) are always welcome!

[1]: {{< relref "2013-05-21-learning-nvp-part-1-high-level-architecture.md" >}}
[2]: {{< relref "2013-08-16-learning-nvp-part-2-nvp-controllers.md" >}}
[3]: {{< relref "2013-08-19-learning-nvp-part-3-nvp-manager.md" >}}
[4]: {{< relref "2013-08-22-learning-nvp-part-4-adding-hypervisors-to-nvp.md" >}}
[5]: {{< relref "2012-11-07-using-vlans-with-ovs-and-libvirt.md" >}}
[6]: {{< relref "2012-11-12-libvirt-ovs-integration-revisited.md" >}}
[7]: {{< relref "2012-08-21-working-with-kvm-guests.md" >}}
