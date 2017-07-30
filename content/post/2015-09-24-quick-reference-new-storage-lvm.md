---
author: slowe
categories: Education
comments: true
date: 2015-09-24T00:00:00Z
tags:
- Linux
- Storage
- CLI
title: A Quick Reference to Adding New Storage with LVM
url: /2015/09/24/quick-reference-new-storage-lvm/
---

This post walks through the process of adding storage capacity to a Linux server using LVM. There's nothing new, revolutionary, or cutting-edge about this post---honestly, it's really more for my own reference than anything else. Adding logical volumes is something that I do so infrequently that it's hard to remember all the commands, so I'm recording them here for when I need them next time.

First, list the physical disks in the system (all commands should be prefaced with `sudo` or run as a user with the appropriate permissions):

    fdisk -l

This will help you identify which (new) disk needs to be added. In my examples, I'll use `/dev/sdb`.

Start partitioning the new disk (replace `/dev/sdb` with the appropriate values for your system):

    fdisk /dev/sdb

I'm assuming that this isn't a boot drive and that whatever logical volumes you create will take up the entire disk. Once you get into `fdisk`, follow these steps:

1. Enter `n` to create a new partition.
2. Enter `p` to make this a primary partition.
3. Enter `1` to make this the first partition on the disk.
4. Press enter twice to accept default start and end cylinders (unless you know you need to change them).
5. Enter `t` to change the partition type.
6. Enter `8e` for the "Linux LVM" partition type.
7. Enter `p` to view the partition info so you can verify it is correct.
8. Assuming the information is correct, enter `w` to write the changes to the disk and exit `fdisk`.

Now, create a new physical volume (replace `/dev/sdb1` with the correct value for your system):

    pvcreate /dev/sdb1

These steps assume you need to create volume group to hold the new physical volume (use `vgextend` instead if you're adding a physical volume to an existing volume group). This example assumes a volume group name of "hdd_vg":

    vgcreate hdd_vg /dev/sdb1

Now create a logical volume in the new volume group. This command creates a 200GB logical volume named "vmstore" in the volume group "hdd_vg":

    lvcreate -L 200G -n vmstore hdd_vg

Before you can use this new logical volume, you'll need to format it (replace `ext4` with your desired file system and replace `/hdd_vg/vmstore` with the correct volume group and logical volume names):

    mkfs -t ext4 /dev/hdd_vg/vmstore

Use the `blkid` command to obtain the volume's UUID for use in `/etc/fstab`:

    blkid /dev/hdd_vg/vmstore

Make a mount point where this new logical volume will be mounted:

    mkdir -p /mnt/vmstore

Edit `/etc/fstab` to specify the UUID of the new logical volume, the mount point, the file system, and other necessary information. Once you're done, mount the new storage with `mount -a`. All done!

**UPDATE**: Reader Rutger van Esch pointed out that it's not necessary to create the partition before creating the physical volume; you can actually create the physical volume directly on the block device using `pvcreate /dev/sdb`. This makes it easier to do online resizing later down the road. Thanks Rutger!
