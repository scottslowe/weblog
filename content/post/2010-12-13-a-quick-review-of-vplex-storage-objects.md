---
author: slowe
categories: Education
comments: true
date: 2010-12-13T09:00:00Z
slug: a-quick-review-of-vplex-storage-objects
tags:
- EMC
- SAN
- Storage
- Virtualization
- VPLEX
title: A Quick Review of VPLEX Storage Objects
url: /2010/12/13/a-quick-review-of-vplex-storage-objects/
wordpress_id: 2175
---

Now that I've managed to get my VPLEX clusters up and running, I'm ready to start providing some posts on VPLEX and how it's configured and used. To get things started, I want to first provide an overview of VPLEX storage objects and how they relate to each other.

**Storage volumes:** Storage volumes are the LUNs that are presented to the VPLEX back-end ports from the storage array. The screenshot below, taken from the VPLEX management GUI, shows a series of storage volumes taken from two separate back-end arrays.

![VPLEX storage volumes](/public/img/vplex-storage-volumes.png)

**Extents:** Extents are created on storage volumes. You have the option of creating multiple extents on a single storage volume, but EMC generally recommends creating one extent per storage volume. (Future posts will discuss why this is beneficial.)

![VPLEX extents](/public/img/vplex-extents.png)

**Devices:** Devices are created from extents. You can combine multiple extents together to form a single device, or you can present a single extent up as a single device. As you can see in the screenshot, devices also have a _geometry_ associated with them, like RAID-0 (no mirroring of devices together), RAID-1 (mirroring of devices), or RAID-C (concatenating devices).

![VPLEX devices](/public/img/vplex-devices.png)

**Distributed devices:** Distributed devices are mirrored devices that are spread across two VPLEX clusters connected together into a metro-plex. Distributed devices require extents from both VPLEX clusters in the metro-plex. Like "regular" devices, distributed devices also have a geometry. As far as I know, all distributed devices will automatically have a RAID-1 geometry. Note that because distributed devices aren't confined to a single cluster, they reside in a different place in the hierarchy.

![VPLEX distributed devices](/public/img/vplex-dist-devices.png)

**Virtual volume:** Virtual volumes are built from devices. When a virtual volume is built from a device, it's referred to just as a virtual volume. When the virtual volume is built from a distributed device, it's referred to as a distributed virtual volume. Virtual volumes are what are exposed (_exported_ in VPLEX parlance) to hosts via the VPLEX front-end ports.

![VPLEX virtual volumes](/public/img/vplex-virt-volumes.png)

In future VPLEX posts, I'll provide information on managing these various storage objects (creating, renaming, and deleting objects) as well as information on integrating VPLEX with other data protection solutions. Stay tuned!
