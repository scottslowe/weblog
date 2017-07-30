---
author: slowe
categories: Information
comments: true
date: 2010-03-11T11:16:25Z
slug: vss-can-cause-problems-with-rm
tags:
- Snapshot
- Virtualization
- VMware
- Windows
title: VSS Can Cause Problems with RM
url: /2010/03/11/vss-can-cause-problems-with-rm/
wordpress_id: 1860
---

This is just a quick post about a potential fix for some timeout issues when using EMC Replication Manager (RM). An e-mail sent to an internal distribution list described a situation in which a user was using RM but was getting an error when trying to take a VMware snapshot. The error reported was a fairly generic error:

>Cannot create a quiesced snapshot because the create snapshot operation exceeded the time limit for holding off I/O in the frozen virtual machine.

As it turns out, the problem was actually VSS in the Windows Server 2003-based guest. Since RM leverages VSS, an error with VSS was causing the entire process to fail. The fix was to clean up VSS as described in [this Microsoft KB article](http://support.microsoft.com/kb/940184) and then reinstall the VMware Tools. After completing both of those steps, the problem was resolved.

If you are using RM and run into this problem, be sure to double-check to ensure that VSS is working as expected.
