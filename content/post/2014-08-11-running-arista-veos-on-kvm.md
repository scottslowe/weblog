---
author: slowe
categories: Tutorial
comments: true
date: 2014-08-11T09:00:00Z
slug: running-arista-veos-on-kvm
tags:
- KVM
- Libvirt
- Linux
- Networking
- OVS
- Virtualization
- Interoperability
title: Running Arista vEOS on KVM
url: /2014/08/11/running-arista-veos-on-kvm/
wordpress_id: 3491
---

In this post, I'll show you how I got Arista's vEOS software running under KVM to create a virtualized Arista switch. There are a number of other articles that help provide instructions on how to do this, but none of those that I found included the use of [libvirt](http://libvirt.org/) and/or [Open vSwitch (OVS)](http://openvswitch.org/).

In order to run vEOS, you must first obtain a copy of vEOS. I can't provide you with a copy; you'll have to register on the Arista Networks site (see [here](https://www.arista.com/en/login)) in order to gain access to the download. The download consists of two parts:

1. The Aboot ISO, which contains the boot loader

2. The vEOS disk image, provided as a VMware VMDK

Both of these are necessary; you can't get away with just one or the other. Further, although the vEOS disk image is provided as a VMware VMDK, KVM/QEMU is perfectly capable of using the VMDK without any conversion required (this is kind of nice).

One you've downloaded these files, you can use the following libvirt domain XML definition to create a VM for running Arista vEOS (you'd use a command like `virsh define <filename>`).

``` xml
<domain type='kvm'>
  <name>veos</name>
  <memory unit='KiB'>2097152</memory>
  <currentMemory unit='KiB'>2097152</currentMemory>
  <vcpu placement='static'>1</vcpu>
  <resource>
    <partition>/machine</partition>
  </resource>
  <os>
    <type arch='x86_64' machine='pc-i440fx-1.5'>hvm</type>
    <boot dev='cdrom'/>
    <boot dev='hd'/>
  </os>
  <features>
    <acpi/>
    <apic/>
    <pae/>
  </features>
  <clock offset='utc'/>
  <on_poweroff>destroy</on_poweroff>
  <on_reboot>restart</on_reboot>
  <on_crash>restart</on_crash>
  <devices>
    <emulator>/usr/bin/qemu-system-x86_64</emulator>
    <disk type='file' device='disk'>
      <driver name='qemu' type='vmdk'/>
      <source file='/var/lib/libvirt/images/eos-4.13.3-veos.vmdk'/>
      <target dev='hda' bus='ide'/>
      <alias name='ide0-0-0'/>
      <address type='drive' controller='0' bus='0' target='0' unit='0'/>
    </disk>
    <disk type='file' device='cdrom'>
      <driver name='qemu' type='raw'/>
      <source file='/var/lib/libvirt/images/aboot-veos-2.0.8.iso'/>
      <target dev='hdc' bus='ide'/>
      <readonly/>
      <alias name='ide0-1-0'/>
      <address type='drive' controller='0' bus='1' target='0' unit='0'/>
    </disk>
    <controller type='usb' index='0'>
      <alias name='usb0'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x01' function='0x2'/>
    </controller>
    <controller type='pci' index='0' model='pci-root'>
      <alias name='pci0'/>
    </controller>
    <controller type='ide' index='0'>
      <alias name='ide0'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x01' function='0x1'/>
    </controller>
    <interface type='network'>
      <source network='br-ex' portgroup='management'/>
      <model type='e1000'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x03' function='0x0'/>
    </interface>
    <interface type='network'>
      <source network='br-ex' portgroup='trunked'/>
      <model type='e1000'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x05' function='0x0'/>
    </interface>
    <interface type='network'>
      <source network='br-ex' portgroup='trunked'/>
      <model type='e1000'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x06' function='0x0'/>
    </interface>
    <serial type='pty'>
      <source path='/dev/pts/4'/>
      <target port='0'/>
      <alias name='serial0'/>
    </serial>
    <console type='pty' tty='/dev/pts/4'>
      <source path='/dev/pts/4'/>
      <target type='serial' port='0'/>
      <alias name='serial0'/>
    </console>
    <input type='mouse' bus='ps2'/>
    <graphics type='vnc' port='5903' autoport='yes' listen='127.0.0.1'>
      <listen type='address' address='127.0.0.1'/>
    </graphics>
    <video>
      <model type='cirrus' vram='9216' heads='1'/>
      <alias name='video0'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x02' function='0x0'/>
    </video>
    <memballoon model='virtio'>
      <alias name='balloon0'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x04' function='0x0'/>
    </memballoon>
  </devices>
  <seclabel type='dynamic' model='apparmor' relabel='yes'/>
</domain>
```

(Click [here](https://gist.github.com/scottslowe/ca31c88a52284eb12ac2) for this XML text as a GitHub Gist.)

There are a few key things to note about this libvirt domain XML:

* Note the boot order; the VM must boot from the Aboot ISO first.

* Both the Aboot ISO as well as the vEOS VMDK are attached to the VM as devices, and you _must_ use an IDE bus. Arista vEOS will refuse to boot if you use a SCSI device, so make sure there are no SCSI devices in the configuration. Pay particular attention to the `type=` parameters that specify the correct disk formats for the ISO (type "raw") and VMDK (type "vmdk").

* For the network interfaces, you'll want to be sure to use the e1000 model.

* This example XML definition includes three different network interfaces. (More are supported; up to 7 interfaces on QEMU/KVM.)

* This XML definition leverages [libvirt integration with OVS][1] so that libvirt automatically attaches VMs to OVS and correctly applies VLAN tagging and trunking configurations. In this case, the network interfaces are attaching to a portgroup called "trunked"; this portgroup [trunks VLANs up to the guest domain][2] (the vEOS VM, in this case). In theory, this should allow the vEOS VM to support VLAN trunk interfaces, although I had some issues making this work as expected and had to drop back to tagged interfaces.

Once you have the guest domain defined, you can start it by using `virsh start <guest domain name>`. The first time it boots, it will take a _long_ time to come up. (A _really_ long time---I watched it for a good 10 minutes before finally giving up and walking away to do something else. It was up when I came back.) According to the documentation I've found, this is because EOS needs to make a backup copy of the flash partition (which in this case is the VMDK disk image). It might be quicker for you, but be prepared for a long first boot just in case.

Once it's up and running, use `virsh vncdisplay` to get the VNC display of the vEOS guest domain, then use a VNC viewer to connect to the guest domain's console. You won't be able to SSH in yet, as all the network interfaces are still unconfigured. At the console, set an IP address on the Management1 interface (which will correspond to the first virtual network interface defined in the libvirt domain XML) and then you should have network connectivity to the switch for the purposes of management. Once you create a username and a password, then you'll be able to SSH into your newly-running Arista vEOS switch. Have fun!

For additional information and context, here are some links to other articles I found on this topic while doing some research:

* [vEOS - Running EOS in a VM](https://eos.arista.com/veos-running-eos-in-a-vm/) (this article provides only limited QEMU/KVM information toward the end)

* [Running vEOS on ESXi 5.5](https://eos.arista.com/running-veos-on-esxi-5-5/)

* [Create a VirtualBox Arista vEOS image from the command line](http://thenetworksherpa.com/create-a-vbox-arista-veos-image-via-command-line/)

* [Building a Virtual Lab with Arista vEOS and VirtualBox](http://www.gad.net/Blog/2012/10/27/building-a-virtual-lab-with-arista-veos-and-virtualbox/)

If you have any questions or need more information, feel free to speak up in the comments below. All courteous comments are welcome!

[1]: {{< relref "2012-11-07-using-vlans-with-ovs-and-libvirt.md" >}}
[2]: {{< relref "2013-05-28-vlan-trunking-to-guest-domains-with-open-vswitch.md" >}}
