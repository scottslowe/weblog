---
author: slowe
categories: Tutorial
comments: true
date: 2009-01-08T12:46:35Z
slug: creating-a-bootable-esxi-usb-stick-on-mac-os-x
tags:
- ESXi
- macOS
- Virtualization
- VMware
title: Creating a Bootable ESXi USB Stick on Mac OS X
url: /2009/01/08/creating-a-bootable-esxi-usb-stick-on-mac-os-x/
wordpress_id: 1127
---

I recently found myself with a decent HP DL385 G2 server with no hard drives (it _used_ to have hard drives, but now it doesn't...there's a long story behind it that I won't get into here). So, I decided I'd try creating a bootable ESXi USB flash drive to use with the server. There are lots of guides out there for creating bootable ESXi USB flash drives, but none of them were written for users, like myself, who use Mac OS X. Telling a Mac user to use WinImage just doesn't work, and while Linux-oriented guides are closer, they still don't address any Mac-specific issues.

So, here's my guide for creating a bootable ESXi USB flash drive from Mac OS X.

1. Download the ESXi installable ISO.

2. Double-click the ISO to mount it (an icon will appear on your desktop). From there, navigate the contents of the ISO image to find `VMware-VMvisor-big-3.5.0-xxxx.i386.dd.bz2` and copy it out of the ISO image into a separate folder.

3. Insert the USB flash drive into an available USB port. Mac OS X will mount the drive and an icon will appear on your desktop.

4. Open the Terminal and type `diskutil list` to list the available drives. On my system, the USB drive was listed as `/dev/disk1`, but your mileage may vary. It should be pretty easy to tell which device is the USB drive, as the first partition (i.e., `/dev/disk1s1`) will have a label that matches the name of the icon on the desktop.

5. Unmount the USB drive by typing `diskutil unmountDisk /dev/disk1`. Replace `/dev/disk1` in the command above with the appropriate entry for your system, as identified in the previous step. The icon for the USB flash drive will disappear from your desktop.

6. Write the image to the USB drive with the command `bzcat <path to VMware-VMvisor file> | dd of=/dev/disk1`. Replace `/dev/disk1` in the command above with the appropriate entry for your system, as identified by the `diskutil list` command in step 4.

When the process completes---you'll know because the Terminal prompt will return---use this command to "eject" the USB flash drive and make it safe for physical removal:

	diskutil eject /dev/disk1

Again, replace `/dev/disk1` with the appropriate device for your system.

At this point, the USB flash drive should be ready to roll. Insert it into a compatible server and virtualize away!

In the process of creating this guide, I found the following sites to be extremely helpful:

[mechanised.com: How to Create Your Own Bootable ESXi USB Stick](http://blog.mechanised.com/2008/07/how-to-create-your-own-bootable-esxi.html)  

[Create ISO on Mac OS X 10.4 | Martin Bergek](http://www.bergek.com/2008/10/28/create-iso-on-mac-os-x-104/)  

[Installing VMware ESXi on a USB memory stick using Ubuntu](http://kuparinen.org/martti/comp/vmware/esxionusb.html)

For Mac users, the special sauce is the `diskutil` command. Unmounting the USB drive from the Finder also made the underlying BSD device, i.e., `/dev/disk1`, disappear. Without unmounting it in Finder, the device is reported as "busy" and can't be accessed (even via root). By using `diskutil`, we are able to make the device accessible.
