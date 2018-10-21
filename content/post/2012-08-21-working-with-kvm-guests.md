---
author: slowe
categories: Education
comments: true
date: 2012-08-21T14:07:34Z
slug: working-with-kvm-guests
tags:
- KVM
- Libvirt
- Ubuntu
- Virtualization
- Windows
title: Working with KVM Guests
url: /2012/08/21/working-with-kvm-guests/
wordpress_id: 2729
---

As you may already know, my new role at EMC has me spending more time on open source projects like KVM, OpenStack, and others. Over the past few days, I've been working with KVM for a bit, and I wanted to share a few things I've learned along the way. Readers who are very familiar with KVM won't likely find anything new here (although I do encourage you to share any additional information or insight in the comments).

The information presented here is based on working with KVM using [libvirt](http://libvirt.org/index.html). For those unfamiliar with libvirt, it is an open source framework/API that can help manage multiple hypervisors and virtual machines. If you use a different management layer---such as [oVirt](http://www.ovirt.org/)--then things might look a bit different, so keep that in mind. (My use of libvirt vs. oVirt is not an endorsement one way or another; I just happened to start with libvirt.) Also, I'm only focusing on command-line tools here; there are a number of graphical user interfaces (GUIs) available as well, which I may investigate further at some point in the future.

## The Components of a KVM Guest

Let's start with looking at the different components of a KVM guest. If you are familiar with VMware virtualization, you know that a VMware-based VM has essentially two components:

* The VM definition, stored in a .VMX file

* The VM's storage, typically stored in one or more .VMDK files

From what I've been able to determine so far, KVM guests also have essentially two components:

* The VM definition, defined in XML format

* The VM's storage, stored either as a volume managed by an LVM or as a file stored in a file system

You can look at the XML configuration of a KVM guest (more properly referred to as a domain) in a couple of different ways. Both involve the use of `virsh`, the command-line tool that is part of libvirt:

* To edit the configuration of a guest, use `virsh edit <Name of guest VM>` and the system will open the XML configuration in your default editor.

* To export the configuration of a guest, use `virsh dumpxml <Name of guest VM>`. This dumps the XML configuration to STDOUT; you can redirect the output to a file if you like.

Here's a snippet of the XML configuration for a KVM guest:

```xml
        <devices>
        <emulator>/usr/bin/kvm</emulator>
        <disk type='file' device='disk'>
          <driver name='qemu' type='raw'/>
          <source file='/var/lib/libvirt/images/vm01.img'/>
          <target dev='hda' bus='ide'/>
          <address type='drive' controller='0' bus='0' unit='0'/>
        </disk>
```

The second component of a KVM guest is the storage; as I mentioned earlier, this can be a file on a file system or it can be a volume managed by a logical volume manager (LVM). The XML snippet above shows the configuration for a disk image stored as a file on a file system. This particular disk image is in QEMU raw format.

With that (very brief) overview complete, here is how you perform some common tasks with KVM guests.

## Creating a KVM Guest

There are a couple of different ways to create a KVM guest:

* Manually create the XML definition of the guest, then use `virsh define <Name of XML file>` to import the definition. You could, naturally, create a new XML definition based on an existing definition and just change a few parameters.

* Use a libvirt-compatible tool, like `virt-install`, to create the guest definition.

Here's a quick example of creating a KVM guest using `virt-install` (I've inserted backslashes and line breaks for readability):

    virt-install --name vmname --ram 1024 --vcpus=1 \
    --disk path=/var/lib/libvirt/images/vmname.img,size=20 \
    --network bridge=br0 \
    --cdrom /var/lib/libvirt/images/os-install.iso \
    --graphics vnc --noautoconsole --hvm \
    --os-variant win2k3

This command creates a KVM guest named "vmname", with 1024 MB of RAM, a single vCPU, a 20 GB virtual disk (in the default QEMU raw format, thin provisioned so that not all 20 GB are allocated up front), connected to an OS installation ISO at the path specified and using hardware virtualization. The network connection is to a bridge called `br0`, which might be a fake bridge on Open vSwitch (which in my case it is). Access to the console is handled via VNC.

Full details on the various parameters for `virt-install` are available via the man page (or you can look [here](http://linux.die.net/man/1/virt-install)).

## Starting and Stopping KVM Guests

This one is easy:

* To start a VM, just run `virsh start <Name of guest VM>` and you're off to the races.

* To stop a VM, run `virsh shutdown <Name of guest VM>` and the guest will start an orderly shutdown. (BTW, I know that the orderly shutdown works for Ubuntu and Windows guests, but I haven't figured out the exact mechanism yet. Any insight there?)

## Changing Guest Configuration

If the VM is shut down, you can use the `virsh edit <Name of guest VM>` command to edit the XML definition directly.

If the VM is running, then only certain things can be done. You are supposed to be able to swap floppy or CD/DVD images using the `virsh qemu-monitor-command` command. In practice I found that it ejected CD/DVD images fine, but wouldn't load a new CD/DVD ISO image. I'm still working on a fix for that one (tips are welcome).

## Using Paravirtualized Drivers

For improved performance, you can also use paravirtualized drivers. (This is the equivalent of using VMXNET3 or PVSCSI in a VMware world.) So far I've only tested the network drivers on Ubuntu and Windows, but they seem to work just fine. (These paravirtualized drivers are referred to as the virtio drivers.)

To use virtio drivers for networking, edit the guest configuration like this (the `model` line is the line that needs to be added):

```xml
    <interface type='bridge'>
      <mac address='52:54:00:a0:55:ef'/>
      <source bridge='br0'/>
      <model type='virtio'/>
    </interface>
```

I found [this page](https://help.ubuntu.com/community/KVM/Networking) to be quite helpful in determining exactly how to enable the virtio network drivers. This part is the same for both Ubuntu and Windows guests; for Windows guests, you also have to install the virtio drivers into Windows. That can be a bit more problematic; I'd recommend copying them across the network _before_ making the change above.

This change must be made while the guest is shut down, by the way.

## Cold Migrations of KVM Guests

You can probably figure out how this one works by now:

1. Shut down the guest using `virsh shutdown <Name of guest VM>`.

2. Export the XML configuration using `virsh dumpxml <Name of guest VM> > <Name of XML file>`.

3. Copy the XML definition you just created and the guest's disk image (specified in the XML configuration) to the target node.

4. Edit the XML configuration (as needed) to reflect any changes in paths.

5. Define the guest on the new node using `virsh define <Name of XML file>`.

Obviously, this assumes an image-based disk, not a LVM-backed disk.

While hardly a comprehensive list of all the various operations that might need to be done with a KVM guest, hopefully this will at least get you started. I'll post more as I learn more, and readers are encouraged to share tips, tricks, or other information in the comments below.
