---
author: slowe
categories: Information
comments: true
date: 2017-04-03T00:00:00Z
tags:
- Productivity
- Linux
- Fedora
- Messaging
- Microsoft
- Storage
- Telephony
- Ubuntu
- Collaboration
title: 'The Linux Migration: Corporate Collaboration, Part 3'
url: /2017/04/03/linux-migration-corp-collab-pt3/
---

In discussing support for corporate communication and collaboration systems as part of my Linux migration, I've so far covered e-mail in [part 1][xref-1] and calendaring in [part 2][xref-2]. In this post, I'm going to discuss the last few remaining aspects of corporate collaboration: instant messaging/chat, meetings and teleconferences, and document sharing.

## Teleconferences and meetings

The topic of teleconferences and meetings is closely related to calendaring---it's often necessary to access your calendar or others' calendars when coordinating meetings or teleconferences---so I encourage you to read [part 2][xref-2] to get a better feel for the  challenges around calendaring/scheduling. All the same challenges from that post apply here. GNOME Calendar, although it offers basic Exchange Web Services (EWS) support, does not support meeting invitations, looking up attendees, free/busy information, etc. This makes it completely unusable for setting up meetings. Evolution provides the backend support that GNOME Calendar uses but may be better suited as a frontend; I haven't tested this functionality so I don't know. This [EWS provider for Lightning][link-4] _does_ support free/busy information, inviting attendees, etc., so it may be a good option (I'm still testing it).

The second aspect of teleconferences/meetings is the actual conduct of the meeting itself. Hosting calls/meetings isn't a large part of my day, although attending them is (pretty typical in a corporate environment of any size). Here's what I've encountered so far:

* WebEx's browser-based interface seems to work reasonably well in Firefox (as a meeting attendee). As far as I know, there is no native WebEx client for Linux. I haven't tested computer audio with WebEx; I've only dialed in (or had it call me).
* GoToMeeting and its variants don't run on Linux at all.
* Skype for Business (S4B)---formerly Lync---is especially challenging, given that there is no native Linux client for S4B. There is [a third-party S4B client][link-2] from a small company called tel.red. My limited experience with the voice side of this product is that voice quality was decent, but entering digits/codes didn't work---thus joining an S4B meeting was impossible since you couldn't enter the conference code. More on this third-party S4B client in the next section.

## Instant messaging/chat

From this particular perspective, how effective you are as a Linux user will greatly depend on your company's IM/chat options. If your employer uses IRC, you're golden---there's a plethora of IRC clients out there (I finally settled on [HexChat][link-5]). If your employer uses XMPP for IM/chat, then you're again golden: [Pidgin][link-6] has great support for XMPP (and a wide variety of other protocols). If your employer uses [Slack][link-8], you're still golden; Slack has a Linux client that (in my experience so far) works just fine.

If, on the other hand, your employer has selected Skype for Business (S4B)---as mine has---then the options aren't so plentiful:

* There's no official client for Skype for Business (S4B) for Linux. So, one way or another, you _will_ be using a third-party client.
* I mentioned in the previous section [the Linux client for S4B from tel.red][link-2]. (The client is called Sky or Sky Linux.) My testing with Sky has been less than stellar, unfortunately. I've encountered search bugs that make it impossible to look up folks from the corporate directory, which means that if the user isn't already in your contacts list then you can't chat with them. (I've opened a support case with tel.red, and I'll update this post with more information once it's available.)
* Pidgin has [a SIPE plugin][link-7] that interoperates with Office 365/S4B. Originally, I thought that there was no way to search for contacts ("buddies" on Pidgin parlance) with the SIPE plugin, but an astute reader showed me that it is, in fact, possible (go to the Accounts menu, select the SIPE (or "Office Communicator") account name, and look for "Contact search...").

My employer uses both S4B as well as Slack (and some other internal tools that are specific to my employer), so it's a mixed bag. I'm using the Sky Linux client as well as the SIPE plugin for Pidgin to try to address the S4B side, and running the Slack client to address the Slack side.

## Document formats

A vast majority of companies out there use Microsoft Office as their standard productivity suite, and therefore rely heavily on the Office document formats. For most use cases, [LibreOffice][link-15] works well enough that exchanging documents with other users is pretty seamless. It will begin to break down as the documents grow more and more complex, so keep that in mind. This appears to be especially prominent with complex PowerPoint presentations. Given that today's corporate world tends to use PowerPoint for everything, keep this in mind.

Note that there are a few other options out there besides LibreOffice (like its forerunner, OpenOffice), but LibreOffice seems to be where the Linux community has converged.

## Document sharing

Clearly, there are plenty of document sharing services out there. From a personal perspective---because very few corporations of which I know use this service---[Dropbox][link-9] works acceptably under Linux. The UI integration is far weaker on [Fedora][link-10] with GNOME than under [Ubuntu][link-11] with Unity, but the underlying sync service works acceptably either way.

From a corporate perspective, a lot will depend on what solution your employer uses. Based on what I've shared with readers so far, it should come as no surprise to learn that my employer selected [OneDrive for Business][link-12] (OD4B) as the preferred corporate document sharing service. It should further come as no surprise to learn that there is _no_ Linux client for OD4B.

Fortunately, not all is lost. There is a sync service called [ODrive][link-3] that allows you to tap into OD4B from Linux, using their Linux Sync Agent. Using ODrive to access OD4B from Linux does actually work (I'm using it), but it's not without its limitations:

* No GUI whatsoever---everything is CLI-driven.
* Sync isn't as "automatic" as you might like; you need to refresh directories for new OD4B files to show up, and then you must manually sync them to your local Linux system. (New files created locally will automatically sync to OD4B.)
* ODrive's free tier does have a few other limitations (can't unsync a file once it's synced, no support for encryption, no support for multiple mount points).

One nice thing about ODrive is that it also allows you to access other document sharing services that corporations may typically use---like [Google Drive][link-14] or [Box][link-13]. Finally, you're able to access _all_ these services at the same time, which is kind of handy.

ODrive is an interesting solution that I'll be discussing more in the future, so keep an eye out for a more in-depth discussion of ODrive.

## Summary

This wraps-up the series on corporate collaboration and communication using Linux. As you can see from reading the posts, some areas are far easier to solve than others, and a lot depends on the corporate solution being used on the backend. I suspect that a great majority of organizations out there are heavily reliant on Microsoft technologies (Exchange, Office, etc.), so using Linux in such environments _might_ be a bit challenging depending on your job role. If your employer has gone "all in" on Office 365 and related services/offerings _and_ you need to often host meetings and calls, then using Linux as your primary desktop OS is probably going to mean keeping a Windows VM running as well. If your employer is also leveraging some other technologies _or_ meetings/calendaring isn't quite as important, then you may be in better shape to adopt Linux as your primary desktop OS. As always, readers should evaluate their situation based on their specific needs and make the decision that is right for them, whether that means Windows, OS X, or Linux.



[link-1]: https://products.office.com/en-us/business/office
[link-2]: https://tel.red/linux.php
[link-3]: https://www.odrive.com/
[link-4]: https://github.com/Ericsson/exchangecalendar
[link-5]: https://hexchat.github.io/
[link-6]: https://pidgin.im/
[link-7]: http://sipe.sourceforge.net/
[link-8]: https://slack.com/
[link-9]: https://www.dropbox.com/
[link-10]: https://getfedora.org/
[link-11]: http://www.ubuntu.com/
[link-12]: https://onedrive.live.com/about/en-us/business/
[link-13]: https://www.box.com/
[link-14]: https://www.google.com/drive/
[link-15]: https://www.libreoffice.org/
[xref-1]: {{< relref "2017-03-21-linux-migration-corp-collab-pt1.md" >}}
[xref-2]: {{< relref "2017-03-27-linux-migration-corp-collab-pt2.md" >}}
