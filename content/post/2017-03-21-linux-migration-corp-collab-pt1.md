---
author: slowe
categories: Information
comments: true
date: 2017-03-21T00:00:00Z
tags:
- Productivity
- Linux
- Fedora
- Messaging
- Exchange
- Collaboration
title: 'The Linux Migration: Corporate Collaboration, Part 1'
url: /2017/03/21/linux-migration-corp-collab-pt1/
---

One major aspect of [my migration to Linux as my primary desktop OS][xref-1] is how well it integrates with corporate communication and collaboration systems. Based on the feedback I've gotten from others on Twitter, this is a major concern for a lot of folks out there. In fact, a number of folks have indicated that this is the _only_ thing keeping them from migrating to Linux. There are a number of different aspects to "corporate communication and collaboration," so I'm breaking this down into multiple posts (each post will discuss one particular aspect). In this post, I'll discuss integration with corporate e-mail.

Because corporate e-mail is such an important part of how people communicate these days, it's a fairly significant concern when thinking of migrating to Linux. Fortunately, it's actually pretty easy to solve.

My employer, like many companies out there, uses [Office 365][link-1] for corporate e-mail. Many people think that this locks them into Outlook on the desktop side, but that's not accurate. (Now, you may be locked into Outlook for other reasons, like calendaring---a topic I'll touch on in part 2 of this series.) For Office 365 users, there are three paths open for accessing corporate e-mail:

1. IMAP/SMTP (yes, O365 _does_ support IMAP/SMTP!)
2. Exchange Web Services (EWS)
3. Browser-based access

Each option has its advantages and disadvantages, naturally:

* If you choose to go down the IMAP/SMTP route, you'll have lots of flexibility in choosing an e-mail client ([Thunderbird][link-2], [Evolution][link-3], [Geary][link-4], and [KMail][link-5], just to name a few).
* _However_, IMAP/SMTP doesn't provide address book lookup (you'd need LDAP support for that), which may make using these protocols in a larger corporate environment a bit more challenging.
* If you choose Exchange Web Services, you have a couple different options from which to choose. If you want to use a client with EWS support, you can use either Thunderbird with [the paid Exquilla add-on][link-6], or Evolution. There's also a package called [DavMail][link-12] that essentially acts like an EWS proxy and will let you use any IMAP client.
* Choosing EWS generally also gives you address book lookups and _maybe_ some calendaring support (again, more on that in part 2).
* Browser-based access is pretty self-explanatory, and the pros and cons are also pretty self-explanatory.

Which route should you choose? Well, that's dependent upon a great many factors, including but not limited to your tolerance for various levels of integration and stability, support within your selected Linux distribution, and personal preference.

I tested Evolution and Thunderbird (both with and without the Exquilla add-on). I did not spend any significant time with any other Linux e-mail clients. Given Evolution's close ties to [GNOME][link-7] and the fact that I'd settled onto [Fedora][link-8] 25 as my Linux distribution of choice, I expected that I would end up using Evolution. Unfortunately, I found Evolution's workflow just didn't sit well with me. Evolution does feel a lot like Microsoft Outlook, which may be beneficial for folks migrating from Windows but didn't help me at all. It also didn't help that Evolution was crash-prone during my testing, or that it occasionally got "bogged down" in background mail tasks and became unresponsive.

Perhaps because of my years spent with Apple Mail, Thunderbird felt far more natural to me, and had the benefit of being cross-platform (meaning I could run Thunderbird on my Mac Pro workstation as well as on my Linux laptop for ease of switching back and forth). Thunderbird's support for "unified folders" (one Inbox across multiple accounts, for example) is still a bit spotty, and so although this was a feature to which I'd grown accustomed on OS X I had to give it up with Thunderbird. The paid Exquilla add-on (which offers a 60-day free trial) works reasonably well, and the cost is only $10/year.

In the end, I selected Thunderbird with the paid Exquilla add-on for Exchange Web Services support, finding that the lack of address book support with an IMAP/SMTP setup was hindering my productivity. To enhance my experience, I also installed a few extra add-ons:

* I added the [Enigmail add-on][link-9] to support digital signing and encryption, tying that together with Keybase and GPG (GNU Privacy Guard).
* I'm using the [Quicktext add-on][link-10] for quickly inserting boilerplate phrases and e-mail signatures.
* The [GNotifier add-on][link-11] ties Thunderbird notifications into the native Fedora/GNOME notifications.

This setup seems to work well, is stable, and doesn't introduce unnecessary friction into my day. (There is some friction there, mostly due to a need to "unlearn" 14 years of Apple Mail habits and keyboard shortcuts.)

In the next post on corporate collaboration, I'll discuss calendaring.



[link-1]: https://products.office.com/en-us/business/office
[link-2]: https://www.mozilla.org/en-US/thunderbird/
[link-3]: https://wiki.gnome.org/Apps/Evolution/
[link-4]: https://wiki.gnome.org/Apps/Geary
[link-5]: https://www.kde.org/applications/internet/kmail/
[link-6]: https://addons.mozilla.org/en-us/thunderbird/addon/exquilla-exchange-web-services/
[link-7]: https://www.gnome.org/
[link-8]: https://getfedora.org/
[link-9]: https://addons.mozilla.org/en-us/thunderbird/addon/enigmail/
[link-10]: https://addons.mozilla.org/en-us/thunderbird/addon/quicktext/
[link-11]: https://addons.mozilla.org/en-us/thunderbird/addon/gnotifier/
[link-12]: http://davmail.sourceforge.net/
[xref-1]: {{< relref "2016-12-16-linux-migration-initial-progress-report.md" >}}
