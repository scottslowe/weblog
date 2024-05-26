---
author: slowe
categories: Tutorial
comments: true
date: 2022-06-01T15:00:00-06:00
tags:
- Fedora
- Firefox
- Linux
- Web
title: Making Flatpak Firefox use Private Browsing by Default
url: /2022/06/01/making-flatpak-firefox-use-private-browsing-by-default/
---

In April 2021 I wrote a post on [making Firefox use Private Browsing by default][xref-1], in which I showed how to modify the GNOME desktop file so that [Firefox][link-1] would open private windows by default without restricting access to normal browsing windows and functionality. I've used that technique on all my [Fedora][link-2]-based systems since that time, until just recently. What happened recently, you ask? I switched to the [Flatpak][link-3] version of Firefox. Fortunately, with some minor tweaks, this technique works with the Flatpak version of Firefox as well. In this post, I'll share with you the changes needed to make the Flatpak version of Firefox also use private browsing by default.<!--more-->

When working with the non-Flatpak version of Firefox, the GNOME desktop file installed with the Firefox package is found at `/usr/share/applications`. In [my earlier article][xref-1], I suggested editing that file to add the `--private-window` parameter to the `Exec` line. Unfortunately, that change gets overwritten every time the Firefox package is updated. It's better, actually, to use a locally customized desktop file placed in `~/.local/share/applications` instead, which will take precedence over the shared desktop file.

With the Flatpak version of Firefox, there is still a shared GNOME desktop file, and support for a locally-customized version as well. The shared desktop file, installed with the Flatpak, is found here on my Fedora system:

```text
/var/lib/flatpak/app/org.mozilla.firefox/current/active/export/share/applications
```

(It's worth noting that the `current` and `active` portions of the directory path are symlinks.)

You _can_ edit the `org.mozilla.firefox.desktop` file at this location to add the `--private-window` parameter to the first `Exec` line, like this (it's all on a single line; I've wrapped it here for you):

```text
Exec = /usr/bin/flatpak run --branch=stable --arch=x86_64 \
--command=firefox --file-forwarding org.mozilla.firefox \
--private-window @@u %u @@
```

However, when the Flatpak gets updated, then this change will get overwritten. Instead, it would be better to copy `org.mozilla.firefox.desktop` from the path above to the corresponding local path:

```text
~/.local/share/flatpak/exports/share/applications
```

Then edit this local desktop file to include the `--private-window` parameter on the first `Exec` line. Making the change to the local copy ensures that the change will not get overwritten or replaced when the Flatpak gets updated.

After you save this change, you'll find that launching the Firefox Flatpak version will open a private browsing window by default. You can still open a regular window as needed, but any links that you click in other applications will automatically open in a private window (or a tab in an existing private window).

There does appear to be one drawback to this approach, and I haven't found a workaround/solution yet. When you edit the local desktop file, you can still launch Firefox as normal, but Firefox disappears as a valid option for the default web browser in the Default Applications screen of the GNOME Settings app. I don't know why this happens.

I hope this information is useful. Feel free to contact me---[on Twitter][link-4], in any one of a variety of Slack communities, or elsewhere online---if you have any questions, comments, or suggestions for improvement.

[link-1]: https://www.mozilla.org/en-US/firefox/
[link-2]: https://getfedora.org/
[link-3]: https://flatpak.org/
[link-4]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2021-04-13-making-firefox-on-linux-use-private-browsing-by-default.md" >}}
