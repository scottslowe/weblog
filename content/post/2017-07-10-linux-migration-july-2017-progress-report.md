---
author: slowe
categories: Information
comments: true
date: 2017-07-10T00:00:00Z
tags:
- Linux
- Fedora
- macOS
- Collaboration
title: 'The Linux Migration: July 2017 Progress Report'
url: /2017/07/10/linux-migration-july-2017-progress-report/
---

I'm now roughly six months into using Linux as my primary laptop OS, and it's been a few months since [my last progress report][xref-3]. If you're just now picking up this thread, I encourage you to go back and read [my initial progress report][xref-1], see [which Linux distribution][xref-2] I selected, or check how I chose to handle corporate collaboration (see [here][xref-4], [here][xref-5], and [here][xref-6]). In this post, I'll share where things currently stand.<!--more-->

My configuration is unchanged from the last progress report. I'm still running [Fedora][link-1] 25, and may consider upgrading to Fedora 26 when it releases (due to be released tomorrow, I believe). I'm still using the Dell Latitude E7370, which continues---from a hardware perspective---to perform admirably. CPU power is a bit limited, but that's to be expected from a mobile-focused chip. My line-up of applications also remains largely unchanged as well.

Some things are working really well:

* [Sublime Text][link-2] runs really well and is quite fast, making it easy to continue using Markdown as my primary content format. Sublime Text's performance and stability have been unparalleled.
* I've had no performance or stability issues with [Firefox][link-3] (for browsing) or [Enpass][link-4] (for password management).
* [ODrive][link-5], while not fully integrated into the Linux desktop environment, has worked well for access to files stored in OneDrive for Business (OD4B).
* [Vagrant][link-7] seems noticeably faster on Fedora than on macOS. I don't use Vagrant as much as I used to, so perhaps this isn't terribly important.

Other things are _not_ working so well:

* Performance in [Thunderbird][link-6] is less than ideal. I see numerous lock-ups and delays while trying to work. I do not see similar issues with other applications, leading me to believe the issue lies within Thunderbird itself.
* Managing my corporate calendar is probably my number one issue right now. As I described [in this post][xref-2], GNOME Calendar is incredibly unstable, Exchange providers for Lightning are spotty (and I have to deal with the Thunderbird performance issues), and other alternatives don't seem to be much better. A number of folks have suggested using Outlook Web Access (OWA), which _is_ an option but not an ideal one (for me)---for one, what about offline access? For an individual who travels a fair amount, offline access to my calendar is a reasonable requirement.
* Integrating into the corporate Skype for Business (S4B) infrastructure is also problematic, though this hasn't been a huge stumbling block (yet).
* I'm running into an increasing number of interoperability issues with Office 365 and [LibreOffice][link-8]. This manifests itself in devious ways, such as subtle color changes, error messages about documents needing to be repaired, and similar. This seems to happen most frequently with presentations. Given that my employer seems to use presentations for almost everything, this is a bit problematic.
* VM performance for my corporate Windows 10 image is a bit less than desirable. I attribute this primarily to the mobile-focused CPU in the Dell E7370 laptop, and not to Fedora, Linux, or the virtualization software.
* Connecting to external displays has been less reliable with the Dell/Fedora combination than on macOS. It's possible that some of this is due to the fact that the E7370 has only a micro-HDMI connector, but even when connecting to HDMI displays (using a micro-HDMI to HDMI dongle) I've seen more failures than when working with Apple's variety of display dongles.

One completely unexpected thing that's arisen is I've noticed an increase in eyestrain since switching to Linux. I don't think this has anything to do with my ergonomic setup (my monitor is still at the same height as before and still the same distance away), but I can definitely tell a difference when I've used my Linux laptop for a while versus going back to macOS. I can only guess it has something to do with color schemes, font rendering, and/or antialiasing, but I haven't been able to put my finger on the culprit yet.

All in all, I'm mostly happy with Linux, but the key areas where I'm unhappy are so important and impactful that it's making life difficult. I must confess that I am considering a move back to macOS, despite my numerous misgivings about the platform. It's not that any one thing is pushing me away; it's more like "death from a thousand cuts"---all the small things are adding up to make quite a significant difference. Add in the unexpected eyestrain, and I have reasonable justification for moving back to macOS.

Does that mean this effort has been a failure? Not entirely; I've learned quite a bit, and anytime you can learn from an effort I don't view it as a failure. (As people we do tend to learn more through failure than success.) I also think it's important to incorporate the concepts of the "blameless port-mortem" and "fail fast" here: figure out what went wrong, examine what worked and what didn't, determine how to correct it, and move forward. It's unclear yet what that means moving forward, but I'll be sure to keep documenting the journey here.

Feel free [to hit me up on Twitter][link-9] if you have thoughts to share about this topic; I'd love to hear your feedback and ideas!

[link-1]: https://getfedora.org/
[link-2]: http://www.sublimetext.com/
[link-3]: https://www.mozilla.org/en-US/firefox/
[link-4]: https://www.enpass.io/
[link-5]: https://www.odrive.com/
[link-6]: https://www.mozilla.org/en-US/thunderbird/
[link-7]: https://www.vagrantup.com/
[link-8]: https://www.libreoffice.org/
[link-9]: https://twitter.com/scott_lowe/
[xref-1]: {{< relref "2016-12-16-linux-migration-initial-progress-report.md" >}}
[xref-2]: {{< relref "2017-02-01-linux-migration-final-distro-selection.md" >}}
[xref-3]: {{< relref "2017-04-12-linux-migration-april-2017-progress-report.md" >}}
[xref-4]: {{< relref "2017-03-21-linux-migration-corp-collab-pt1.md" >}}
[xref-5]: {{< relref "2017-03-27-linux-migration-corp-collab-pt2.md" >}}
[xref-6]: {{< relref "2017-04-03-linux-migration-corp-collab-pt3.md" >}}
