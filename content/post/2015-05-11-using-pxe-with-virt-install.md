---
author: slowe
categories: Information
comments: true
date: 2015-05-11T07:13:00Z
tags:
- Linux
- KVM
- Virtualization
- CLI
title: Using PXE with virt-install
url: /2015/05/11/using-pxe-with-virt-install/
---

In this post, I'll just share a quick command that can be used to build and install a KVM guest using PXE instead of an ISO image. There's nothing new here; this is just me documenting a command so that it's easier for me (and potentially others) to find next time I need it.

I shared how to use the `virt-install` command to build KVM guest domains in a blog post talking about [working with KVM guests][xref-1]. In that post, I used an ISO image with the `virt-install` command to build the guest domain.

However, there may be times when you would prefer to use PXE instead of an ISO image. To build a KVM guest domain and instruct the guest domain to boot via PXE, you would use this command (I've inserted backslashes and line returns to improve readability):

	sudo virt-install --name=guest-name --ram=2048 --vcpus=1 \  
	--disk path=/var/lib/libvirt/images/guest-disk.qcow2,bus=virtio \  
	--pxe --noautoconsole --graphics=vnc --hvm \  
	--network network=net-name,model=virtio \  
	--os-variant=ubuntuprecise

The key here is the `--pxe` parameter, which `virt-install` uses to instruct the guest domain to PXE boot instead of booting from a virtual CD-ROM backed by an ISO image.

Naturally, you'd want to substitute the desired values for the KVM guest domain name, the vCPUs and RAM allocated to the guest domain, the path to the disk image (here specified as a QCOW2 file), the network name, and the OS distribution.

Happy PXE booting your KVM guests!


[xref-1]: {{< relref "2012-08-21-working-with-kvm-guests.md" >}}
