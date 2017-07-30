---
author: slowe
categories: Explanation
comments: true
date: 2007-05-15T13:07:30Z
slug: resizing-disks-inside-guests
tags:
- Hardware
- Linux
- Virtualization
- VMware
title: Resizing Disks Inside Guests
url: /2007/05/15/resizing-disks-inside-guests/
wordpress_id: 456
---

While I would love to be able to say that this procedure I'm going to describe for resizing disks inside guest VMs is new or unique or original, I can't. I'm sure that lots of smart people out there have been down this path before, and more than a couple of them have probably written up good instructions on how to do it. I'm including this information here partly for myself (in the event I need it in the future), and partly because the information fits in with a lot of the other information I have here on [VMware](http://www.vmware.com/) and related technologies.

So, with that being said, here's how I recently went about resizing some guest VM disks using `vmkfstools` and [GParted](http://gparted.sourceforge.net/index.php) (specifically, the [GParted LiveCD](http://gparted.sourceforge.net/livecd.php)). This process assumes you need to resize a Windows guest. The process should be very similar if not the same for non-Windows guests, but I haven't tested it so I can't be absolutely certain. The process is very straightforward and not unusual in the least, but feel free to post any corrections or questions in the comments.

1. Download the GParted LiveCD (here's a [direct link](http://sourceforge.net/project/showfiles.php?group_id=115843&package_id=173828)) and then upload it to your [ESX Server](http://www.vmware.com/products/vi/esx/) using the file upload tool of your choice. Being a [Mac OS X](http://www.apple.com/macosx/) user, I use [Interarchy](http://www.nolobe.com/interarchy/). Use whatever best suits you.

2. Power off the VM that you will be manipulating. (I know, that seems obvious, but still...)

3. Open an SSH session to the ESX Server where your guest VM is hosted. Switch to root, change into the appropriate directory where the VM is stored, and then run the command `vmkfstools -X <New disk size> <VMDK filename>`. For example, if your current virtual hard disk was 10GB and you wanted to make it 20GB, then specify "20g" on the command line, i.e., specify the new total size of the disk, _not_ the amount by which to increase it.

4. Flipping to the graphical VI Client, change the CD-ROM of the guest VM to be the ISO image of the GParted LiveCD you downloaded earlier. Make sure it is set to connect at power on. Power on the VM and boot from the CD.  

Note that booting from the CD can be a bit tricky. You may need to boot up several times before you catch it just right. _Be sure_ that if Windows starts booting up, you let it boot up and then shut it back down again. If you reset the VM in the midst of the Windows boot sequence, the NTFS filesystem will be marked as "dirty" and GParted won't make the changes you wanted.

5. GParted will boot up from the CD. You may need to press Enter a couple of times to accept the defaults (unless you need settings other than the defaults, of course) before the graphical environment loads and you see the GParted interface. Once the GParted interface is up, you should be able to figure out how to make the changes you want to make.

6. Shutdown the VM (you can use the shutdown option in GParted), disconnect the CD-ROM, and power the VM back up again. When Windows boots, it will run a Chkdsk, then reboot again, and then come up to a login screen. After you login, you may be prompted to reboot again after the discovery of "new hardware." After that final reboot, you should be good to go with a new, larger virtual hard disk.

The most time consuming portion, in my experience, is waiting for GParted to boot from the ISO image. Otherwise, the entire process is almost completely painless.
