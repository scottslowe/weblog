---
author: slowe
categories: Tutorial
comments: true
date: 2014-03-17T09:00:00Z
slug: modifying-qcow-images-with-guestfish
tags:
- CLI
- Linux
- OpenStack
- Virtualization
title: Modifying QCOW Images with Guestfish
url: /2014/03/17/modifying-qcow-images-with-guestfish/
wordpress_id: 3423
---

In this post, I'm going to talk about a way to modify QCOW2-based images without having to boot up a full-blown virtual machine based on the image. I found this process while trying to change the behavior of cloud-init on [the official Ubuntu cloud images](http://cloud-images.ubuntu.com) from Canonical.

While I had successfully built my own Ubuntu 12.04 LTS image for my [OpenStack](http://openstack.org)-based home lab (and it worked just fine), I was keen to use Canonical's official cloud images as they were much smaller (around 250MB versus 1.5GB for my home-built image). Given that disk space is at a bit of a premium in my home lab, using the smaller images would be quite beneficial. The problem was that cloud-init wasn't configured to operate in a way that worked for my home lab environment.

The question, of course, washow do I do this? I tried a couple different approaches:

1. I installed VirtualBox on one of my OS X systems, because I knew VirtualBox supported QCOW2 images. Unfortunately, trying to launch a VM using one of the official cloud images as a root disk failed. (I presume this is because the cloud images assume the use of the virtio block driver, which VirtualBox doesn't support.)

2. Next, I installed a plain jane Ubuntu VM in VirtualBox and attached the official cloud image QCOW2 file as a second hard disk. That didn't work either; the Ubuntu VM refused to recognize the filesystem(s) on the QCOW2 image.

I was about at my wits' end when I found [this post](https://ask.openstack.org/en/question/5531/defining-default-user-password-for-ubuntu-cloud-image/). Aha! I'd heard of `guestfish` before, but I hadn't thought to try it in this instance. Guestfish is part of the larger [libguestfs project](http://libguestfs.org).

So I tried the instructions in the post I'd found. Unfortunately, it failed the first time I tried, but I later---thanks to `qemu-img check`---found that the Ubuntu 12.04 cloud image I'd downloaded was somehow damaged/corrupted. Using a new download of the image was successful.

Here are the steps I used to modify the contents of a QCOW2 disk image using `guestfish`:

1. Starting from a vanilla Ubuntu 12.04 LTS install, use `apt-get update` and `apt-get install guestfish` to install guestfish. (You'll probably need/want to use `sudo` with both of these commands.)

2. During the installation of guestfish, you can select to create a "virtual appliance" that guestfish uses. If you choose not to do this during installation, you can always do it later with `sudo update-guestfs-appliance`.

3. Once guestfish is installed and the virtual appliance is created/updated, launch guestfish itself with `sudo guestfish`. Note that if you don't have root privileges or don't use `sudo`, guestfish will still launch and act like everything is working---but it will fail. Once guestfish is up and running, you'll be sitting at a "> &lt;fs&gt;" prompt.

4. Type `add <QCOW2 image filename>` (obviously substituting the full path and correct filename for the QCOW2 image you'd like to modify).

5. Type `run` and press Enter. You'll see a progress bar that updates the status of the guestfs virtual appliance. Once it's complete, you'll be returned to the "> &lt;fs&gt;" prompt.

6. Enter `list-filesystems`. Guestfish should return a list of filesystems on the QCOW2 disk image. In the case of the official Canonical cloud images, only "/dev/vda1" is returned.

7. To gain access to the filesystem in the QCOW2 image, enter `mount <device specification> <guestfish mount point>`. For example, when I was modifying the Canonical cloud images, I used `mount /dev/vda1 /`. Keep in mind you're not mounting to the system on which guestfish is running; you're mounting to the guestfish virtual appliance.

8. Now just edit the files in the QCOW2 image using whatever method you prefer. In my case, I needed to edit `/etc/cloud/cloud.cfg`, so I just used vi to edit the file and make whatever changes were needed.

9. When you're done, type `exit` to quit guestfish and commit changes to the QCOW2 image.

And that's it! Using this technique I was able to quickly and easily modify the Ubuntu cloud images so that cloud-init behaved in the way that I wanted.

Hopefully this information helps someone else. If you have any questions or corrections, I invite you to share in the comments below.
