---
author: slowe
categories: Rant
comments: true
date: 2007-01-18T17:55:01Z
slug: networking-diagramming-on-the-mac
tags:
- macOS
- Microsoft
- Office
- VMware
- Networking
title: Networking Diagramming on the Mac
url: /2007/01/18/networking-diagramming-on-the-mac/
wordpress_id: 403
---

I spent the entire day trying to create a professional looking network diagram for a customer who recently installed an iSCSI-based SAN (a Network Appliance storage system, by the way). I didn't want generic rectangles and boxes; I wanted nice icons. Preferably vendor-specific icons. Is that so much to ask?

I visited [Graffletopia](http://graffletopia.com/), which is to [OmniGraffle](http://www.omnigroup.com/applications/omnigraffle/) (I use [OmniGraffle Professional](http://www.omnigroup.com/applications/omnigraffle/pro)) what [Visio Cafe](http://www.visiocafe.com/) is to [Visio](http://www.microsoft.com/office/visio/). Unfortunately, I wasn't able to find very many helpful stencils.

Realizing that OmniGraffle Pro (OGP) reads/writes Visio XML files, I thought then that I might be able to export Visio stencils into a form that I could use on my Mac. Alas, no; OGP wouldn't read them. Finally, I settled into manually creating my own OGP stencils from selected items in the Visio stencils, and was finally able to piece together a diagram that was decent. At some point I may post the OGP stencils I'm creating for my own use out on Graffletopia for others as well, provided the original author is amenable to the idea.

In the meantime, I'll continue plugging away at laboriously converting Visio stencil items to OGP stencil items. Here's the process I'm using:

1. Place a single item from a Visio stencil onto a blank Visio diagram and save that diagram as a PNG image.

2. Move the PNG image to my Mac and copy the contents of the PNG to the clipboard.

3. Paste the image into a stencil in OGP. Tweak the size, connection points, etc., until I'm satisfied.

4. Repeat as needed.

Given that VMware Fusion's ability to drag-and-drop from the guest back to the host isn't working (Did it ever work? Or am I imagining things?), step 2 above is more laborious than it should be. Oh well, it could be worse.

Is there a faster process for this? Anyone know?
