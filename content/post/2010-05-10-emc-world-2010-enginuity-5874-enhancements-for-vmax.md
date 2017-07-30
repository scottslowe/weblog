---
author: slowe
categories: Liveblog
comments: true
date: 2010-05-10T10:56:11Z
slug: emc-world-2010-enginuity-5874-enhancements-for-vmax
tags:
- EMC
- EMCWorld2010
- Storage
- Symmetrix
title: 'EMC World 2010: Enginuity 5874 Enhancements for VMAX'
url: /2010/05/10/emc-world-2010-enginuity-5874-enhancements-for-vmax/
wordpress_id: 1934
---

This is a liveblog of the session titled "Performance Improvements and Enhancements of Enginuity 5874 for Symmetrix VMAX: What's New Since Last Year". The presenter's name is John Aurin. (Note: the product branding is changing from "V-Max" to "VMAX".)

The session starts with a recap of the Symmetrix VMAX architecture: the directors, front-end ports, the Virtual Matrix connectivity for sharing data between directors, and the various expansion cabinets and drives that are supported. VMAX supports EFDs (200 or 400GB), 15K Fibre Channel drives (146GB to 600GB), 10K Fibre Channel drives (400GB to 600GB), and SATA drives (1TB).

The discussion now shifts to front-end connectivity on the VMAX. For mainframe connectivity, the VMAX supports zHPF for high-performance FICON connectivity. The use of zHPF for high-performance FICON connectivity improves performance (up to 214% vs. the previous-generation DMX4). 8Gb Fibre Channel support is also available with the latest VMAX software.

Some best practices for front end port layout:

* Go wide across the "A" ports before going deep across the "B" ports.

* Mix different systems across the A and B ports.

* Be careful to provide enough ports when doing frame consolidation.

* Use general recommendations for FICON on VMAX on a port level.

Next the speaker moves into SRDF software compression. This is off by default, but can be enabled and can be turned on for all SRDF modes. It's typically not recommended for SRDF/S, but is recommended for SRDF/A or Adaptive Copy. The choice to leave software compression off by default is typically because organizations deploy network optimization technologies. It cannot be enabled via SYMCLI; it must be enabled via inline command only.

Cloning is the next topic that the speaker discusses. The latest release of the code supports asynchronous copy on first write with virtual provisioning. This offers a performance improvement versus the previous behavior.

Fully Automated Storage Tiering (FAST) is the next topic to be discussed. The current version of FAST works only on full LUNs and is based on Symmetrix Optimizer. FAST uses multiple algorithms to make the swap/move decisions. It is supported for mainframes on the VMAX. FAST does lock the frame briefly, but it does not hold the lock for the duration of the move/swap. The presenter shows a few graphs and reports that show how FAST handles hot drives or high drive utilization by leveraging EFDs or fast Fibre Channel drives for more demanding workloads.

The latest Symmetrix code release supports up to four daisy chain bays added with four engines. Two engines can now support 1200 drives; four engines can support up to 2400 drives. There is no additional latency on the extra drive bays.

Now the discussion moves to zero reclamation. This is only applicable in a virtual provisioning environment. Zero reclamation is a mechanism to return (de-allocate) thin extents that contain all zeros. Zero reclamation is administrator-initiated via SYMCLI. Zero reclamation is probably a one-time event, unless the host OS on the systems attached to the VMAX have the ability to write zeros (like Sdelete).

Both zero reclamation and write leveling are background tasks and don't affect replication, snaps, front-end workloads, etc.

Write leveling is used to rebalance allocation and usage across devices when new devices are added to a virtual provisioning pool.

The session wraps up with some general Enginuity performance enhancements from GA to the current code release:

* 20% improvement in throughput between 5874 GA and 5874 2Q10 SR (these improvements are due to increased use of cache locality)

* Significant enhancements in rebuild times for RAID 5 (3+1), RAID 5 (7+1), and RAID 6 (6+2) configurations from GA to latest code release

With that, the speaker wrapped up the session with a summary of the various topics discussed: SRDF software compression, FAST, 8Gg FC/FICON support, Zero Reclaim, Write Leveling, and Enginuity improvements (faster rebuild, more snaps, enhanced throughput).
