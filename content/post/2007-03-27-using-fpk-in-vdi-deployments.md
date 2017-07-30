---
author: slowe
categories: Explanation
comments: true
date: 2007-03-27T10:58:20Z
slug: using-fpk-in-vdi-deployments
tags:
- ESX
- VDI
- Virtualization
- VMware
- Windows
title: Using FPK in VDI Deployments
url: /2007/03/27/using-fpk-in-vdi-deployments/
wordpress_id: 435
---

Many [VMware](http://www.vmware.com/) [Virtual Desktop Infrastructure](http://www.vmware.com/solutions/desktop/vdi.html) (VDI) deployments will likely be environments where the virtual desktops will be the only desktops that the user ever logs into. Environments like outsourced development (developers use hosted desktops so that the source code never leaves the company's equipment) and call centers are two examples that come to mind. A recent project in which I was involved, though, was a bit different: the customer wanted to use hosted desktops to provide access to a specific application (an application which was very data-intensive), but all other applications would be run from traditional "fat" PCs. Users would be logged in to both a traditional PC as well as a hosted desktop session, typically at the same time.

The customer couldn't use more mainstream server-based computing solutions such as [Citrix Presentation Server](http://www.citrix.com/English/ps2/products/product.asp?contentID=186&ntref=hp_nav_US) or [Microsoft Windows Terminal Services](http://www.microsoft.com/windowsserver2003/technologies/terminalservices/default.mspx) because the application wasn't supported in those environments. It had to be a desktop environment. To complicate the matter, the business needs of the organization dictated that users logging into these hosted desktops needed to have some of their settings travel with them (sort of like roaming profiles), but---and here's the catch--_only when logging in to a hosted desktop session._ The settings the user configured inside the hosted desktop session should move between hosted desktops, but should not affect their traditional PC login.

That's right: the saved settings should only apply when they were logging in to a hosted desktop session, not a regular desktop session. That meant we couldn't use roaming profiles to these settings travel between hosted desktops, since that attribute is set on a per-user account basis in Active Directory and would affect logons to both traditional PCs as well as hosted virtual desktops. Now things start to get interesting.

Fortunately, a tool designed for multi-user environments such as Citrix and Terminal Services was able to save the day. That tool, the [Flex Profile Kit](http://portal.loginconsultants.nl/forum/index.php?board=16;action=display;threadid=1144) (FPK), allows administrators to selectively save portions of a user's profile to a simple file, which can then be reapplied at next logon. Using a configuration file and a small executable, we can define groups of settings that should be saved to a file. That file is stored on a central file share, and then copied back down and re-applied when the user next logs in again. Using this functionality, we can mimic the effect of a roaming profile without having to modify any user objects in Active Directory (and thus limiting the impact to hosted desktops only).

In the following sections I discuss some of the specific challenges we faced when using FPK for this project.

## Mandatory Profiles

Normally, an FPK deployment requires the use of mandatory profiles. Otherwise, the FPK is unable to fully configure the environment to account for "deleted" or "removed" settings, such as a drive mapping that no longer exists or a printer that has been uninstalled. The only way I can describe this is to say that the FPK only stores the settings that should be saved; it doesn't store settings that should be removed or overwritten. As a result, changes in the user's profile that result in the removal of data from the Registry or file system---such as disconnecting a mapped network drive---don't get captured and continue to show up in future sessions. Mandatory profiles fix that because _nothing_ gets saved to the base profile, so only what's in the FPK saved settings will get applied.

However, we couldn't assign mandatory profiles in this case because mandatory profiles are also roaming profiles, and we were under strict mandate that the hosted desktops and the physical desktops were not to affect one another. Try as I may, I couldn't think of any way to assign roaming profiles without those settings also affecting logons to physical systems.

We were finally able to circumvent that problem by using REG.EXE, from one of the Windows Resource Kits, to delete the Registry keys associated with the values that were being stored by FPK. At logoff, the logoff script would use FPK to store the values in the user's profile, then REG.EXE would "clean" those values out of the user's profile. When the user logged in again, he or she would receive only the values stored by the FPK.

## Running FPK at Logon/Logoff

Again, due to the fact that we could not allow the hosted desktops to share settings with the physical desktops, we also couldn't run FPK through the user's regular login script (again, the attribute is specified via Active Directory and would affect logons to both physical and hosted desktops). This means we needed a way to run FPK at logon and logoff, but only for hosted desktops. Fortunately, this challenge was fairly easy to overcome: use a Group Policy object (GPO) on the OU where the hosted desktop computer accounts are found, enable loopback processing, and set the logon/logoff scripts there.

For those that aren't familiar with loopback processing, here's what it is (in a nutshell). Normally, Active Directory would process the computer settings portion of a GPO based on the location of the computer object in Active Directory, and apply the user settings portion of a GPO based on the user's location in AD. However, in some cases, we might want to apply user settings based on the _computer's_ location, such as when a user is logging into a particular machine or particular type of machine. This situation is a prime example of just such a case. We want user settings applied only when the user logs into a hosted desktop, so we include the user settings in a GPO that is linked the OU where the computer accounts for the hosted desktops reside and enable loopback processing (in "Replace" mode, in this instance). Then, those user settings will only apply when the user logs into one of those systems, and not any other systems. Problem solved.

If you are currently considering a VDI project, I'd encourage you to have a look at FPK. You may find it useful, as I did.

**UPDATE:** Michael Roth of [thincomputing.net](http://www.thincomputing.net/) has provided an [updated URL for the FPK](http://www.loginconsultants.com/index.php?option=com_docman&task=doc_details&gid=1&Itemid=62).  Thanks, Michael!
