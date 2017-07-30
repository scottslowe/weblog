---
author: slowe
categories: Information
comments: true
date: 2008-11-03T12:04:30Z
slug: p2v-with-ibm-blades
tags:
- Hardware
- IBM
- Virtualization
- VMware
title: P2V With IBM Blades
url: /2008/11/03/p2v-with-ibm-blades/
wordpress_id: 1010
---

A colleague and a customer passed me some information late last week about performing physical-to-virtual (P2V) conversions on with blades in an IBM blade chassis. (This was with an older BladeCenter chassis, not the BladeCenter H chassis.)

Apparently, if you run a P2V on a blade in the chassis without first selecting the blade on the chassis' KVM, then the resulting virtual machine does not have a CD-ROM drive. Users will have to shutdown the VM, add the CD-ROM drive to the VM, and then boot it back up again.

I'm guessing this is because access to the chassis' shared CD-ROM drive is handled via the KVM. In any case, keep this in mind if you are performing P2V operations with older IBM blades.

This behavior was noticed using VMware Converter. I don't have any information on whether other P2V products are similarly affected, but I suspect that they would be.

Thanks to Chauncey and Keith for the heads-up.
