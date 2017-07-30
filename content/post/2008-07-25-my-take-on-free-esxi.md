---
author: slowe
categories: Musing
comments: true
date: 2008-07-25T17:00:55Z
slug: my-take-on-free-esxi
tags:
- ESX
- ESXi
- Virtualization
- VMware
title: My Take on Free ESXi
url: /2008/07/25/my-take-on-free-esxi/
wordpress_id: 769
---

There's been a lot of coverage about VMware's announcement that VMware ESXi would be free starting Monday, July 28, 2008. More than a couple of people asked me why I haven't said anything about it yet, and to be honest it was because I was waiting until I could offer something more than just "Hey, ESXi will be free!"

I'm still not so sure that I have anything valuable to add beyond what has already been discussed extensively elsewhere on the Internet, but I thought I would at least weigh in on the subject.

First, I'm not surprised. Informal discussions I'd had with various VMware resources had hinted that this was on the way; besides, it's the natural response to a major competitor who is, for all intents and purposes, releasing their product for free. So this move isn't surprising in the least.

Second, I _would_ be surprised if this was only Maritz' doing. I suspect that Diane Greene had this plan in the works for months before her sudden departure. Seems like I saw mention somewhere of a confirmation of this, but I can't find it now. If anyone knows of such a confirmation, I'd appreciate a link in the comments.

Third, you have to remember that VMware is only releasing ESXi for free. They're not releasing VirtualCenter for free. You'll still need VirtualCenter and VI3 Enterprise licenses in order to do stuff like VMotion, Storage VMotion, VMware DRS, VMware HA, VMware DPM, etc. Just like Microsoft, whose System Center Virtual Machine Manager and the rest of the System Center suite will be "paid-for" products, VMware will continue to charge for VirtualCenter. However, keep in mind that the APIs that VirtualCenter uses are widely available, so there's nothing stopping anyone from writing their own free (perhaps open source?) replacement for VirtualCenter. Somehow, I can't see that happening with SCVMM or any of the other members of the System Center suite.

This move makes ESXi the "gateway drug" (pardon the comparison) to full-blown VMware Infrastructure. Get the light stuff for free and get you hooked, then charge you for the heavy stuff. It's a tried-and-true practice that almost every software vendor out there uses. In my opinion, the arrival of this model to the virtualization market is merely another indicator of the market's maturity. This move will begin to shake out the virtualization wannabes who don't have the strength or stamina to duke it out with the bigger players.
