---
author: slowe
categories: Liveblog
comments: true
date: 2008-06-12T13:53:45Z
slug: vir350-system-center-vmm-advanced-integration
tags:
- HyperV
- Microsoft
- TechEd2008
- Virtualization
- Windows
title: 'VIR350: System Center VMM Advanced Integration'
url: /2008/06/12/vir350-system-center-vmm-advanced-integration/
wordpress_id: 740
---

Yes, yet another System Center VMM session...it's pretty clear that System Center is a major component of Microsoft's server virtualization strategy. This session is VIR350, System Center VMM Advanced Integration, so I suppose we will be seeing more PowerShell and more integration with other System Center family members. As with the other liveblogged sessions, I'll try my best to weed out duplicate content.

The presenter for this session is David Armour, a Senior Program Manager at Microsoft.

(Side note: what exactly _is_ a Program Manager, anyway? Microsoft must have thousands upon thousands of them. I think that every single presenter so far this week has been a Program Manager or a Senior Program Manager.)

The focus of this session will be on how to extend or customize System Center VMM, and most of the information presented here will apply to both VMM 2007 and VMM 2008 (currently in beta). The key technology used in this case is PowerShell, which can be used either against Hyper-V directly or against VMM. VMM, however, vastly simplifies the PowerShell code required to perform a task when compared to doing the same task against Hyper-V directly.

As has been stated elsewhere, VMM is built on PowerShell, and the GUI represents only a subset of all the functionality of the overall feature set available via PowerShell. Note that the self-service web portal is also built on top of PowerShell. David goes on to discuss the various ways in which a client, like the VMM GUI, interacts with the PowerShell layer.

David then moves into a demo of VMM. He walks through the creation of a new VM, and one thing I noticed that I hadn't seen before was the idea of a "hardware profile." This is a set of hardware properties like number of CPUs, amount of RAM, number of NICs, etc. This is a nice feature, as it separates common hardware configurations from the OS installation. Typical VM templates combine the hardware configuration and the OS installation together.

In the demo, David shows how the automatically-generated PowerShell script can be easily modified to use a variable and prompt the user for information so that you can create a script that quickly and easily creates a new virtual machine with the name of your choice. That's fairly handy.

The next few slides described the hierarchical nature of the VMM PowerShell objects, and how the PowerShell Cmdlets always generate a job in VMM. This allows VMM to audit jobs, provide a job history, and store changes invoked by a job. Security can also be applied to a job, so as to enforce ACLs. This also allows long-running jobs to be asynchronously monitored over time via the job.

David recommends using the PowerShell button in VMM; this automatically loads the appropriate snap-in so that all the VMM Cmdlets are available for use. He then launches into a fairly in-depth demo and review of PowerShell, how to interrogate a snap-in to determine its commands, how to sort or filter output to show only the desired results, how to view the details on a particular command, and how to use some simple pipes. He also showed some ways to get more information or help or to view detailed documentation on a command or a command's parameters.

The next little while was spent walking through a series of scenarios of using PowerShell to perform various tasks. First is a series of tasks to provide a report (or a group of reports) to management. Next David walks through scenarios involving the creation of new VMs, including creating a hardware profile, attaching hardware, and using intelligent placement for the new VM.

Tired of the boring old PowerShell command prompt? David moves into a demo of [PowerGUI](http://www.powergui.org/), a way of turning PowerShell commands into a GUI application. He also demonstrated PowerGadget Creator, which allows one to create a Windows Vista Sidebar Gadget using PowerShell. This would allow users to create tools to display VM or VM host information in the Vista Sidebar. Finally, David shows how to use Visual Studio to extend VMM using PowerShell. Frankly, this level of extensibility and customization is probably beyond most users, but I suppose it's useful functionality to have nevertheless.

The next topic was....(drum roll please)....PRO! That's right, another discussion of the integration between VMM and Operations Manager which is built upon PowerShell. Fortunately, David didn't spend a great deal of time covering PRO yet again (thank you!).

David closed out the session with a quick summary of the material covered and pointed attendees to a few online resources. I found the session reasonably helpful, even if only from the perspective of getting more familiar with the VMM object model so that I can write my own PowerShell scripts.
