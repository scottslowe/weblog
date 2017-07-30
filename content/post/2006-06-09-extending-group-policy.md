---
author: slowe
categories: Information
comments: false
date: 2006-06-09T17:13:11Z
slug: extending-group-policy
tags:
- Microsoft
- ActiveDirectory
- Firefox
- Interoperability
- Macintosh
- UNIX
- VMware
- Windows
title: Extending Group Policy
url: /2006/06/09/extending-group-policy/
wordpress_id: 265
---

Two articles recently surfaced amid the wave of information that deluges me daily, and both were regarding the extension of Group Policy---a key feature of Active Directory---into areas that were previously untapped.

First, there came the announcement of the [FrontMotion Firefox Community Edition](http://www.frontmotion.com/Firefox/fmfirefox.htm), a version of [Firefox](http://www.mozilla.com/firefox/) that is deployable and configurable via Group Policy in Active Directory. With this build of Firefox, organizations can deploy Firefox and centrally manage Firefox settings via Group Policy, much in the same way that organizations can centrally deploy and manage Internet Exploder, er, Explorer. (Sorry about that.) Given that the lack of central configuration control over Firefox is one key sticking point to many organizations deploying Firefox en masse, this is good news.

Second, I saw the announcement of Group Policy for [Mac OS X](http://www.apple.com/macosx/), from [Centrify](http://www.centrify.com/) and their [DirectControl for Mac OS X](http://www.centrify.com/directcontrol/mac_os_x.asp) product. DirectControl extends Group Policy to allow Macintosh-specific settings to be controlled centrally via Active Directory. In addition, it also provides single sign-on (SSO) to Active Directory from Macintosh systems. Of course, we know that we can achieve SSO to AD from a Mac today without DirectControl, but DirectControl also gives us the Group Policy functionality as well. As a side note, it's also worth mentioning that Centrify offers [DirectControl for Samba](http://www.centrify.com/directcontrol/samba.asp) (enabling Windows users to seamlessly authenticate to UNIX Samba shares), [DirectControl for VMware ESX Server](http://www.centrify.com/directcontrol/vmware_esx.asp) (for automatic provisioning of ESX Server accounts; not terribly useful in deployments using VirtualCenter), and [DirectControl for ADFS](http://www.centrify.com/directcontrol/adfs.asp) (Active Directory Federation Services). Pretty neat stuff, although I haven't had the chance to see it in action yet.

These announcements show that independent software vendors are now becoming comfortable and knowledgeable enough about Active Directory to begin building these kinds of add-on products to extend the usefulness of Active Directory to non-Windows platforms.
