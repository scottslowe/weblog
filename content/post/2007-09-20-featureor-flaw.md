---
author: slowe
categories: Rant
comments: true
date: 2007-09-20T12:06:33Z
slug: featureor-flaw
tags:
- Microsoft
- Security
- Windows
title: Feature....or Flaw?
url: /2007/09/20/featureor-flaw/
wordpress_id: 547
---

In the end, I'll leave it to the readers to decide whether this functionality I uncovered today (which is not entirely secret, _per se_, just not widely discussed) is "feature" or "flaw". I'm kind of split on the issue myself.

Here's the story. I'm working on a VDI (virtual desktop infrastructure) project for a customer, and this application the customer uses to help manage their desktops (both physical and virtual) kept popping up a security warning dialog box every time a user logged in. We needed a way to suppress the warning so that the dialog didn't keep coming up over and over as users roamed within the pool of available desktops. What's stranger is that it only seemed to happen on some desktops, but not others, even when the systems were absolutely identical. We were baffled.

Finally, this minor comment in [Microsoft KB889815](http://support.microsoft.com/?id=889815) got me to thinking:

>This behavior is new in Windows XP SP2 because of the addition of the Attachment Execution Services (AES). Every program that is run by using the ShellExecute() API passes through AES. AES considers the downloaded update file to be from the Internet Zone. Therefore, AESdisplays the Open File - Security Warning dialog box. AES examines the file to see whether the file has a file stream of the type Zone.Identifier. Then AES determines what zone the file is from and what level of protection to apply when the file is run.

Ah...file streams! I'd learned long ago (when reading _Inside the Windows NT File System_, by Helen Custer) about alternate data streams. The idea is that a single file in NTFS can store different streams of data. At the time, I advocated the use of data streams for use by Office, such as for storing different versions of the file. Unfortunately, Microsoft never seemed to capitalize on the idea...until recently.

The situation here is that when you download a file from the Internet using Internet Explorer, IE automatically creates an alternate data stream named "Zone.Identifier". The contents of this alternate data stream are this:

	[ZoneTransfer]
	ZoneID=3

I'm not sure when this behavior started (with which version of Internet Explorer and/or which version of Windows), but I do know---according to the KB article above---that as of Windows XP Service Pack 2, the presence of this alternate data stream helps Windows determine what kind of security policy to apply to the file. What does this mean? Let me walk through it with you:

1. You download an executable file from the Internet. You are familiar with the site from which you are downloading this file, but the site is in the Internet Zone (Zone 3).

2. When you download the file, IE automatically attaches the alternate data stream and embeds the zone information in this file.

3. You copy this executable to a location on your local hard drive and you run it. You don't expect to be prompted for anything, since the executable is running from your local hard drive.

4. Windows checks the alternate data stream, sees the embedded IE security zone, and applies a security policy appropriately---in this case, popping up a security dialog box prompting the user to confirm the action.

Now, forever for the life of this file (at least as long as it remains on an NTFS file system), it will have the IE security zone embedded with the file and Windows will apply security policies based on that information.

So, in this particular case, this company had downloaded this executable from the vendor's site, then distributed it to hundreds of PCs throughout the organization. Presumably, somewhere along the way some of them lost the alternate data stream (perhaps they were copied to a non-NTFS partition at some point; this organization did have Novell also), and those were the ones that weren't popping up the security warning. Those that _did_ have the alternate data stream had the warning that the user had to acknowledge---despite the fact that the executable was running from the user's local C: drive.

To fix it, we used the [Streams utility](http://www.microsoft.com/technet/sysinternals/FileAndDisk/Streams.mspx) from Sysinternals (now part of the Borg...er, Microsoft collective). This tool allowed us to identify and remove the alternate data stream.

So here's where this discussion turns to the title: is this a feature, or a flaw? Clearly, Microsoft intended this as a security feature. I could see tagging the file on the system where the file was downloaded, but having the alternate data stream follow the file throughout the network (for the duration of the file's life, as long it remains on NTFS) seems just crazy. Not to mention that Microsoft provides _zero_ ways to identify, track, or remove alternate data streams. That's right---there's no way to find or track alternate data streams on a single PC, much less find or track alternate data streams across a bunch of PCs on a network.

So again I ask: feature, or flaw? I'd love to hear your thoughts.

Oh, yes, I almost forgot---for more information on identifying alternate data streams or viewing the contents of alternate data streams, see [this site](http://www.heysoft.de/nt/ntfs-ads.htm).

**UPDATE:** I forgot to mention that Firefox does not add the alternate data stream to files downloaded from the Internet. This makes sense, of course, given that Firefox doesn't use the idea of "security zones" and is designed to be a cross-platform browser. Nevertheless, I thought it important to point this out.
