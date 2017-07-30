---
author: slowe
categories: Tutorial
comments: true
date: 2009-03-25T11:59:11Z
slug: next-gen-stuff-verifying-the-sha-1-fingerprint
tags:
- CLI
- ESX
- ESXi
- Security
- SSL
- Virtualization
- VMware
- vSphere
title: 'Next-Gen Stuff: Verifying the SHA-1 Fingerprint'
url: /2009/03/25/next-gen-stuff-verifying-the-sha-1-fingerprint/
wordpress_id: 1254
---

This post is not necessarily specific to next-generation ESX/ESXi and vCenter Server, but it was prompted by behaviors in these products. (Besides, the truth is that I'm really just trying to be sensationalist and capitalize on interest in the next-generation products.)

When you add an ESX/ESXi host to vCenter Server in the next generation of products, you will receive a security warning that displays the SHA1 thumbprint (or fingerprint) of the ESX/ESXi host's default SSL certificate. The fact that the dialog box displays the SHA1 fingerprint got me to thinking---how does one go about verifying the SHA1 fingerprint to ensure that the host to which you are connecting is really the host you think it is? I mean, that's the idea behind displaying the fingerprint, isn't it? Paranoid people will then go to the specific host in question, generate the fingerprint on the SSL certificate, and then compare the two fingerprints to make sure they are identical.

I haven't figured out a way to do this for ESXi yet, but for ESX you can verify the SHA1 fingerprint of the SSL certificate using this command:

	openssl x509 -sha1 -in /etc/vmware/ssl/rui.crt -noout -fingerprint

This should all be on a single line; it might be wrapped here. The command will then display the SHA1 fingerprint on the SSL certificate, which you can compare to the fingerprint displayed in the vCenter Server dialog box to ensure that the two values match. (If you're really paranoid, you'll run this command at the server's physical console and not remotely. Unless, of course, you took the time to actually verify the SSH key fingerprints when you first connected via SSH, but that's an entirely different post.)

So, here's the real question: how does one verify the SHA1 fingerprint for an ESXi host? The ideal solution should not require the use of any unsupported hacks. (And yes, I know that you can view the SSL certificate, and thus the SHA1 fingerprint, by connecting to the ESXi host remotely using a web browser. But you still don't know for sure that the host to which you connected is the host you thought it was, do you?)

**UPDATE:** At the ESXi console, logging in and selecting the "View Support Information" menu item will display the SSL fingerprint. Challenge solved!
