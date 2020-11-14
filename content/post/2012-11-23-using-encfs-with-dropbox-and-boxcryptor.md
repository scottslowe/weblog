---
author: slowe
categories: Tutorial
comments: true
date: 2012-11-23T10:03:20Z
slug: using-encfs-with-dropbox-and-boxcryptor
tags:
- Encryption
- macOS
- OSS
- Storage
- Interoperability
title: Using EncFS with Dropbox and BoxCryptor
url: /2012/11/23/using-encfs-with-dropbox-and-boxcryptor/
wordpress_id: 2952
---

Lots of folks like using [Dropbox](http://www.dropbox.com/), the ubiquitous store-and-sync cloud storage service; I am among them. However, concerns over the privacy and security of my data have kept me from using Dropbox for some projects. To help address that, I looked around to find an open, interoperable way of adding an extra layer of encryption onto my data. What I found is described in this post, and it involves using the open source EncFS and OSXFUSE projects along with an application from [BoxCryptor](http://www.boxcryptor.com/) to provide real-time, client-side AES-256 encryption.

## Background

First, some background why I went down this path. Of all the various cloud-based services out there, I'm not sure there is a service that I rely upon more than Dropbox. The Dropbox team has done a great job of creating an almost seamlessly integrated product that makes it much easier to keep your files accessible across locations and devices.

Of course, Dropbox is not without its flaws, and security and privacy are considered among the prime concerns. Dropbox states they use _server-side encryption_ to protect your data on the Amazon S3 infrastructure, but Dropbox also controls those server-side encryption keys. Many individuals, myself among them, would prefer _client-side_ encryption with control over our own encryption keys.

So, a fair number of companies have sprung up offering ways to help fix this. One of these is BoxCryptor, who offers an application for Windows, Mac, iOS, and Android that performs _client-side_ encryption. From the Mac OS X perspective, BoxCryptor's solution is, as far as I know, built on top of some fundamental building blocks:

* The open source [OSXFUSE project](http://osxfuse.github.com/), which is a port of FUSE for Mac OS X

* A Mac port of the open source EncFS FUSE filesystem

I would imagine that ports of these components for other operating systems are used in their other platforms, but I don't know this for certain. Regardless, it's possible to use BoxCryptor's application to get client-side encryption across a variety of platforms. For those who want a quick, easy, simple solution, my recommendation is to use BoxCryptor. However, if you want a bit more flexibility, then using the individual components can give you the same effect. I chose to use the individual components, more for my own understanding than anything else, and that's what is described in this post.

## What You'll Need

This post was written from the perspective of getting this solution running on Mac OS X; if you're using a different operating system, the specifics will quite naturally be different (although the broad concepts are still applicable).

There are four main components you'll need:

* OSXFUSE: This is a port of FUSE to OS X, and is one of a couple of successors to the now-defunct MacFUSE project. OSXFUSE is available to download [here](https://github.com/osxfuse/osxfuse/downloads).

* Macfusion: Macfusion is a GUI to help simplify and automate the mounting of filesystems. While it's not strictly necessary, it does make things a lot easier. Macfusion can be downloaded [here](http://macfusionapp.org/).

* EncFS: You'll need a version of EncFS for Mac OS X. There are a variety of ways to get it; I used an installer actually made available by BoxCryptor [here](http://blog.boxcryptor.com/encfs-174-installer-for-mac-os-x-available).

* EncFS plugin for Macfusion: This is what enables Macfusion to mount or unmount EncFS filesystems, and is actually included in the EncFS installer above. You can also download the plugin [here](https://thenakedman.wordpress.com/encfs/).

## Setting Things Up

Once you have all the components you need, then you're ready to start installing.

1. First, install OSXFUSE. When installing OSXFUSE, be sure to select to install the MacFUSE Compatibility Layer. The OSXFUSE installer recommends rebooting after the installation, but I waited until I'd finished installing all the components.

2. Once OSXFUSE is installed, install Macfusion. Macfusion is distributed as a ZIP file; simply unzip the file and move it to the location of your choice. I installed it to /Applications.

3. Next, run the EncFS installer. During the installation, select to install **only** EncFS and the EncFS plugin for Macfusion. Do not install any of the other components. I rebooted here.

4. You'll need both a mount point as well as a directory to store the raw, encrypted data. Since the raw, encrypted data is intended to be synchronized via Dropbox, you'll want to create the encrypted directory in the Dropbox hierarchy. I chose to use `~/Dropbox/Secure`. For the mount point, I chose to use `~/.Secure`. You can obviously modify both of these directories to better suit your own needs or preferences.

5. Once you have all the components installed and the mount point and encrypted directories created, you're ready to actually create the encrypted filesystem. Run the command `encfs ~/Dropbox/Secure ~/.Secure`. The `encfs` program will run through some questions; select "x" for Expert mode and configure it according to the guidelines described in [this support article](https://boxcryptor.desk.com/customer/portal/articles/565934-can-boxcryptor-mount-encrypted-volumes-created-with-encfs-). When prompted for a passphrase, be sure to enter an appropriately complex passphrase---and make sure you remember it (you'll need it later).

6. When `encfs` finishes running, it will mount an encrypted volume on your desktop. It will have an odd name, but you won't be able to change it. Go ahead and eject (unmount) this volume; we'll remount it again shortly using Macfusion. Note that you might see some Dropbox activity here.

7. Launch Macfusion, then re-add the encrypted filesystem created in step 5; you'll need to supply the same passphrase you entered earlier. Here in Macfusion you'll be able to specify a name for the encrypted filesystem and supply a custom icon as well. Mount the encrypted filesystem to be sure that everything is working as expected.

That's it---any files you now copy into the encrypted filesystem---which is represented by an external drive on your Desktop---will be encrypted using AES-256 and then synchronized to Dropbox. Cool, huh?

## Adding Another Computer

I have two Macs in my office (my 13" MacBook Pro and my Mac Pro), so I had to repeat the process on the second Mac so that it could read the encrypted files. If you have more than one computer, you'll need to do the same. Simply go through steps 1 through 5. In step 5, though, it will only prompt for the passphrase. You can even skip steps 5 and 6 to go straight to 7. As long as you have the passphrase for the encrypted filesystem, adding access for additional Dropbox-linked computers should be a piece of cake.

## Adding Access from iOS

This is where BoxCryptor comes back into play again. Install the BoxCryptor app onto your device, then link it to your Dropbox account and select the directory within Dropbox where the raw, encrypted data is found. As long as you followed the configuration guidelines [here](https://boxcryptor.desk.com/customer/portal/articles/565934-can-boxcryptor-mount-encrypted-volumes-created-with-encfs-), BoxCryptor should be able to decrypt the encrypted filesystem created with EncFS.

Following these instructions, you'll gain a way to add AES-256 encryption to your Dropbox files (or a subset of your Dropbox files) while still maintaining access to those files from just about any location across a variety of devices.

If anyone has any questions or clarifications about what I've posted here, please speak up in the comments below. All courteous comments are welcome!
