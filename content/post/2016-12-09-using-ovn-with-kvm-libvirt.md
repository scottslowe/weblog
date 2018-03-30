---
author: slowe
categories: Tutorial
comments: true
date: 2016-12-09T00:00:00Z
tags:
- Libvirt
- Linux
- Virtualization
- Networking
- KVM
- CLI
- OVN
- OVS
title: Using OVN with KVM and Libvirt
url: /2016/12/09/using-ovn-with-kvm-libvirt/
---

In this post, I'm going to discuss how to use OVN (Open Virtual Network; part of [the Open vSwitch project][link-1]) with KVM and Libvirt to provide virtual networking for KVM-based virtual machines. This post will build on some concepts around OVS and Libvirt that I've discussed previously; be sure to review the [OVS posts][xref-2] and [Libvirt posts][xref-1] on this site for more details and prerequisite knowledge.

I'll structure this discussion around 2 key steps:

1. Setting up OVN
2. Integrating KVM/Libvirt into OVN

Note that I'm not going to discuss setting up KVM/Libvirt, as that's something I've covered previously and is well-documented.

Ready? Let's jump in!

## Setting up OVN

The biggest "challenge" here is package availability---many Linux distributions don't have packages available for OVS 2.6.0, which is the first release with non-experimental support of OVN. If you're an Ubuntu user, then you can use the Ubuntu Cloud Archive for the OpenStack "Newton" release, which includes OVS/OVN 2.6.0 packages. For other distributions, you'll probably need to compile from source. In that case, the [OVS installation documentation][link-4] is quite accurate and usable.

For the purposes of this post, I'll assume you're using Ubuntu 16.04 and will pull packages from the Ubuntu Cloud Archive. Instructions for using the Ubuntu Cloud Archive are [here][link-5]; once you've added the UCA repository for Newton, installing OVS and OVN is just a matter of running `apt install` (or `apt-get install`) to install the following packages:

* openvswitch-common
* openvswitch-switch
* ovn-common
* ovn-host

On only *one* system, you'll also want to install the "ovn-central" package. Make note of the IP address where this package is installed; you'll need it later. I'll refer to the IP address of the system where "ovn-central" is installed as CENTRAL_IP.

Naturally, since we're discussing the use of OVN with KVM/Libvirt here, you'd also want to install the packages for those features as well ("qemu-kvm" and "libvirt-bin", for example).

Once the packages are installed, then it's necessary to configure OVN appropriately. Fortunately, this is pretty straightforward:

1. First, stop OVN's local component (the "ovn-controller" component) with this command:

        /usr/share/openvswitch/scripts/ovn-ctl stop_controller

2. Next, you'll need to configure the local OVN controller to talk to OVN's central components. You'll do this by setting some values in the OVS database using `ovs-vsctl`. To set the correct values, run this command:

        ovs-vsctl set Open_vSwitch . \
        external_ids:ovn-remote="tcp:CENTRAL_IP:6642" \
        external_ids:ovn-nb="tcp:CENTRAL_IP:6641" \
        external_ids:ovn-encap-ip=<IP address on this system> \
        external_ids:ovn-encap-type=geneve

    So, what does all this do? You're populating the "external_ids" column of the Open_vSwitch database with values that configure OVN. The "ovn-remote" and "ovn-nb" values tell OVN's local component (a piece called "ovn-controller") how to talk to OVN's central component (running on the sytem where you installed the "ovn-central" package). The "ovn-encap-ip" tells OVS/OVN what IP address to use for overlay tunnels, and the "ovn-encap-type" tells OVS/OVN what encapsulation protocol to use (Geneve, in this case, but others are supported).
 
3. Restart the local OVN controller:

        /usr/share/openvswitch/scripts/ovn-ctl start_controller

At this point, OVN should be up and running. You can verify this by running `ovs-vsctl show`, and you should see that OVN has automatically performed some configuration on the local OVS instance, including setting up Geneve tunnel ports to the other hypervisors in the OVN environment.

Now you're ready to look at integrating KVM/Libvirt with OVN.

## Integrating KVM/Libvirt with OVN

"Integrating" is perhaps the wrong word to use here; there is no pre-built integration between KVM/Libvirt and OVN. You'll have to perform manual steps or build your own custom scripts to make these two projects work together (for now). As I go through this section, I'm going to assume you'll create a Libvirt network to "front-end" your OVS bridge (as described [here][xref-3]). Since we are working with OVN here, you'll want to make sure the Libvirt network references the "br-int" bridge (the default OVN integration bridge).

One part of the Libvirt-OVS integration that is critical to making it work with OVN pertains to the _interface ID_. This is a value that is written into both the OVS database and the VM's Libvirt XML definition. For example, after launching a VM using Libvirt-OVS integration, you can see the interface ID in the Libvirt XML code using `virsh dumpxml <domain>` (the excerpt below shows only the network interface section of the XML):

``` xml
<interface type='bridge'>
  <mac address='52:54:00:93:05:25'/>
  <source network='ovs' bridge='br-int'/>
  <virtualport type='openvswitch'>
    <parameters interfaceid='668033d2-06a4-4aaa-aca5-30cde245e477'/>
  </virtualport>
  <target dev='vnet0'/>
  <model type='virtio'/>
  <alias name='net0'/>
  <address type='pci' domain='0x0000' bus='0x00' slot='0x03' function='0x0'/>
</interface>
```

Note the `parameters interfaceid` element. Now, let's look at the corresponding OVS port (which we know to be "vnet0" from the XML configuration above, as specified by `target dev=vnet0`) using `ovs-vsctl list interface vnet0`:

``` text
_uuid               : 1948c2bc-c768-4b77-9275-78923eae80bd
admin_state         : up
bfd                 : {}
bfd_status          : {}
cfm_fault           : []
cfm_fault_status    : []
cfm_flap_count      : []
cfm_health          : []
cfm_mpid            : []
cfm_remote_mpids    : []
cfm_remote_opstate  : []
duplex              : full
error               : []
external_ids        : {attached-mac="52:54:00:93:05:25", iface-id="668033d2-06a4-4aaa-aca5-30cde245e477", iface-status=active, vm-id="b427e283-f0a1-460b-84a8-56d7ced81b1e"}
ifindex             : 9
ingress_policing_burst: 0
ingress_policing_rate: 0
lacp_current        : []
link_resets         : 1
link_speed          : 10000000
link_state          : up
lldp                : {}
mac                 : []
mac_in_use          : "fe:54:00:93:05:25"
mtu                 : 1500
mtu_request         : []
name                : "vnet0"
ofport              : 3
ofport_request      : []
options             : {}
other_config        : {}
statistics          : {collisions=0, rx_bytes=1437, rx_crc_err=0, rx_dropped=0, rx_errors=0, rx_frame_err=0, rx_over_err=0, rx_packets=9, tx_bytes=648, tx_dropped=0, tx_errors=0, tx_packets=8}
status              : {driver_name=tun, driver_version="1.6", firmware_version=""}
type                : ""
```

Note the `iface-id` value in the "external_ids" column; it matches the `interfaceid` value from Libvirt.

So why is this important? Well, as [the OVS integration guide][link-6] points out, the `iface-id` value is used by OVS integrations to uniquely identify a VM's interface. When integrating KVM/Libvirt/OVS with OVN, we'll need to use this value as well; this provides an "end-to-end" link between an OVN logical port, an OVS virtual port, and a VM's network interface.

With that bit of background out of the way, let's look at what's required to connect a KVM-based VM, using Libvirt and OVS, to OVN:

1. Power on the VM and connect it to OVS. (This is all handled by Libvirt and the Libvirt-OVS integration.)
2. Create an OVN logical port on an OVN logical switch.
3. Populate the addresses associated with the OVN logical port.

Since step #1 is handled by Libvirt, I won't discuss it in any detail here, focusing instead of #2 and #3.

Assuming that an OVN logical switch has already been created (you can use `ovn-nbctl ls-add <switch-name>` on the sytem where the OVN central components are running), adding a logical switch port is pretty straightforward---just use `ovn-nbctl lsp-add <switch name> <port name>`. There's a small "gotcha" though. Remember the discussion about the interface ID? Here's where it comes back to affect us: _the logical switch port you create in OVN **must** use the interface ID as the port name._

To extract the interface ID from OVS, you can use this command:

    ovs-vsctl get interface <name> external_ids:iface-id

So, if the VM was attached to interface "vnet0", then the command would look like:

    ovs-vsctl get interface vnet0 external_ids:iface-id

Now, you might think you could use Bash command substitution to store the output of that command into a variable and use it later, and you'd be _mostly_ correct. `ovs-vsctl` returns the value surrounded by double quotes, and OVN requires the value not have any quotes. So, if you wanted to extract a value you could use in a Bash script, you'd have to do a little manipulation:

    ovs-vsctl get interface vnet0 external_ids:iface-id | sed s/\"//g

Let's say you store that value in a variable named IFACE_ID. To create an OVN logical port that matches the OVS virtual port, you'd use this command:

    ovn-nbctl lsp-add <switch name> $IFACE_ID

OK, that takes care of creating the OVN logical port on an OVN logical switch. Using the interface ID links the OVN logical port back to OVS, which is in turn linked (via the Libvirt XML) to the VM's network interface using the same value.

Next, you'll need to tell OVN which addresses are associated with that logical port. OVS already has this information, but OVN needs it in order to create logical flows for traffic to move from VM to VM. We can extract the necessary information from OVS using this command:

    ovs-vsctl get interface <name> external_ids:attached-mac

To use this information with OVN, though, we'll have to strip the double quotes away. Here's how to do that, and store the output into a variable for use later:

    MAC_ADDR=$(ovs-vsctl get interface vnet0 external_ids:attached-mac | sed s/\"//g)

We can now use that value to populate the addresses on the OVN logical port:

    ovn-nbctl lsp-set-addresses $IFACE_ID $MAC_ADDR

You can verify that the addresses are linked to the logical ports using `ovn-nbctl show`, and you can verify that the logical ports are linked to OVS using `ovn-nbctl list Logical_Switch_Port $IFACE_ID` (the "up" value should be "true", meaning that the link to OVS is working).

At this point, you should be able to enter one of the VMs running on one of the hypervisors and have connectivity to the other VMs on the same logical switch (regardless if they are on the same hypervisor or a different hypervisor).

You now have Libvirt-managed KVM-based VMs connected to OVN via OVS. There's so much more you could do from here---link multiple OVN logical switches together via logical routers, add containers to the same logical switch for seamless VM-container connectivity, control traffic via ACLs---but I'll save those for future blog posts.

## Additional Resources

There's quite a bit of information out there about OVN (see [Russell Bryant's web site][link-3], for example), but not much about using OVN with KVM/Libvirt. To help make it easier for readers to work with this sort of setup, I've created a Vagrant-based environment. This Vagrant-based environment is found in [my GitHub "learning-tools" repository][link-2], in the `ovn-kvm` directory.


[link-1]: http://openvswitch.org/
[link-2]: https://github.com/scottslowe/learning-tools
[link-3]: https://blog.russellbryant.net/
[link-4]: http://openvswitch.org/support/dist-docs/INSTALL.rst.html
[link-5]: https://wiki.ubuntu.com/OpenStack/CloudArchive
[link-6]: http://openvswitch.org/support/dist-docs/IntegrationGuide.rst.html
[xref-1]: /tags/libvirt/
[xref-2]: /tags/ovs/
[xref-3]: {{< relref "2012-11-12-libvirt-ovs-integration-revisited.md" >}}
