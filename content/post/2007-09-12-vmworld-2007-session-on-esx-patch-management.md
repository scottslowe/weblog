---
author: slowe
categories: Liveblog
comments: true
date: 2007-09-12T14:53:26Z
slug: vmworld-2007-session-on-esx-patch-management
tags:
- ESX
- Virtualization
- VMware
- VMworld2007
title: VMworld 2007 Session on ESX Patch Management
url: /2007/09/12/vmworld-2007-session-on-esx-patch-management/
wordpress_id: 537
---

I'm getting ready to start another session (another super-session) on ESX patch management, presented by Brian Gallob. Incidentally, I met Brian on Monday night at the VMTN Communities get-together around the corner at the Thirsty Bear (username is BrianG on the forums). I think I heard there's about 1200 people registered for this session over in Room 103 of Moscone South, the same room used yesterday for the VC bullet-proofing session I attended.

Brian Gallob started out the session by setting some expectations; he's not going to make any significant product announcements in this session, and while VMware is working on something to help ease the patching problems, that solution is yet to be announced. With that expectation in place, Brian proceeded with the session.

Everyone agrees that patching is a necessary evil. Of course, some users (incorrectly) assume that ESX Server doesn't require patches, but that's a false assumption. ESX Server updates include bug fixes, new drivers, driver enhancements, etc., all of which can be valuable to the end users.

In contrast to earlier patching mechanisms, with ESX Server 3 patches are much more specific, users can "pick and choose" which updates are applicable, and there is (at most) a single reboot required. Many updates do not even require a reboot. There is also the new esxupdate tool to help with patching.

Patch notification occurs via e-mail to the e-mail address used during registration of the license; there is also notification posted to the VMTN and the downloads site at VMware (available at [http://www.vmware.com/download/vi/vi3_patches.html](http://www.vmware.com/download/vi/vi3_patches.html)). Patches are classified into three categories:

* Security: These patches fix one or more security vulnerabilities, usually in the Service Console. They should be installed as quickly as possible.

* Critical: These fix a bug or flaw in the software. The flaws that these patches address may cause data loss or severe service disruptions, and should be installed right away.

* General: Ususally a fix for a minor flaw that might affect a small subset of customers. These patches should tested and, if applicable to your environment, installed.

Red Hat packages should never be installed on ESX Server. Although the Service Console is based on Red Hat Enterprise Linux, Red Hat patches won't work and should be avoided.

The new download site (see the URL earlier) now contains a great deal more information that will make it easier to determine if a particular patch applies to your installation. The information available there includes:

* Download size

* Brief description of the patch and a patch number (the patch number links back to a KB article about the patch)

* Impact (reboot required?)

* Supersedes or "superseded by" information

All of this information can be used to determine if general patches are applicable. Security patches and critical patches are recommended to be installed as soon as possible.

With the release of ESX Server 3.0.2, patch installation order is no longer important. With earlier versions, however, patch should be installed in order _by release date_. Within a release date (say, all the patches released in June), order is not important. The June patches should be installed before the July patches, however.

What's in an update tarball? Some RPMs and header/descriptor information used by `esxupdate` (see below).

Some handy `esxupdate` commands are:

* `esxupdate query` - Shows a list of all installed updates. Can use a "-l" (that's a lowercase "L") to show additional information

* `esxupdate info <updatename>` - Shows information about a particular installed update; again, use "-l" (lowercase "L") for additional information

* `esxupdate update <updatename>` - Installs the specified update; can use the "-noreboot" option to prevent a reboot, useful when installing multiple patches at the same time

For additional information, the `/var/log/vmware/esxdupate.log` file contains additional information appended to the log every time esxupdate does something. Tools such as grep (a perennial favorite in these parts, as regular readers will know) can be used to pick up particular pieces of information. Brian particularly liked these two commands:
    
    grep -i summary esxupdate.log  
    grep -i totals esxupdate.log

These commands will show more pertinent information on the patches installed, summary information, how many were installed and how many failed, etc.

To use repositories (instead of local update files), simply add the "-r" parameter with the location of the repository, like so:
    
    esxupdate -r file:/path/to/filename update  
    esxupdate -r http://http server name/path/updatename update  
    esxupdate -r ftp://ftp server name/path/updatename update

Brian also suggested the use of a shared VMFS3 LUN to store update files. That's a pretty handy idea.

Finally, we wrapped up the session with some general troubleshooting tips. Some useful tidbits from the final portion included:

* Don't use the "-f" option---even if recommended to do so by `esxupdate` itself---unless you do a lot of pre-work. If you do, you'll need to go back and re-apply patches. This problem is typically caused by installing patches out of order.

* If you get errors trying to untar the update, verify the MD5 sums to ensure the file integrity has not been damaged. There are Windows-based utilities to calculate the MD5 sum.

* Upgrade to version 2.0.2 of VirtualCenter _before_ you upgrade any ESX hosts to version 3.0.2. Upgrading to 3.0.2 is recommended as it removes the patch installation order requirements.

Finally, Brian mentioned an upcoming session at 3:30 PM about the future product VMware Update Manager. I'd love to attend that session, but unfortunately I'm already booked in another session. I need about 3 more of me around here!
