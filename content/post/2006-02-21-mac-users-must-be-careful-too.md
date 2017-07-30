---
author: slowe
categories: Rant
comments: false
date: 2006-02-21T14:10:18Z
slug: mac-users-must-be-careful-too
tags:
- Macintosh
- Security
title: Mac Users Must Be Careful Too
url: /2006/02/21/mac-users-must-be-careful-too/
wordpress_id: 190
---

After all the hubbub around the new Mac malware subsided, it turned out that Mac users shared something with their Windows-using counterparts: they have to be careful, too.

The extensive coverage of the Mac malware showed that, in the end, the security vulnerability is really more of a user issue than anything else. For more information, see [this coverage from ONLamp.com](http://www.oreillynet.com/pub/wlg/9222?wlg=yes) (part of O'Reilly) or [this vulnerability from Secunia](http://secunia.com/advisories/18963).

For years, Windows users have been preached at not to open executable files, not to open untrusted file attachments, etc. Mac users, living in their smug little world, didn't have to worry about that. "[Mac OS X](http://www.apple.com/macosx/) is immune to viruses," everyone would say.

Hardly. (Yes, for the record, I am a Mac user, and I love my [PowerBook](http://www.apple.com/powerbook/) and Mac OS X.) While I do believe that the core architecture of Mac OS X does lend itself to be generally more secure than Windows, that does _NOT_ mean that Macs are immune. It just means that Mac users need to pay a little more attention to what they are doing, and they need to learn the same lessons that Windows users have had to learn. What lessons are those? Don't open executable attachments. Don't open or download untrusted files. Don't assume that just because a file says it's a picture it's actually a picture.

I will say that [Apple](http://www.apple.com/) must assume a portion of the blame here. First, the whole resource fork/extension mess that the Finder uses to determine how to open an application is partly at fault here. Otherwise, it wouldn't be possible to create an executable file that presented itself as a JPG or an MP3. Apple needs to resolve that, somehow, as things move forward. Second, [Safari](http://www.apple.com/macosx/features/safari/) should have had the "Open safe files after downloading" option turned off by default, rather than the opposite. And, finally, Apple needs to make sure that Mac OS X relies on something other than the "shebang" line to identify files as a shell script.

In the meantime, just use the features already present in your Mac web browser (both Safari and [Camino](http://www.caminobrowser.org/) have options for not opening "safe" files after downloading) and don't blindly trust that files are indeed what they claim to be. It may be a new lesson for most Mac users, but it's an important one for all of us to learn.
