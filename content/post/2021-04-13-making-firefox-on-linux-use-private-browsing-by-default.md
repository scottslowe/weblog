---
author: slowe
categories: Tutorial
comments: true
date: 2021-04-13T16:15:00-06:00
tags:
- Firefox
- Linux
- Web
title: Making Firefox on Linux use Private Browsing by Default
url: /2021/04/13/making-firefox-on-linux-use-private-browsing-by-default/
---

While there are a couple different methods to make [Firefox][link-2] use private browsing by default (see [this page][link-1] for a couple methods), these methods essentially _force_ private browsing and disable the ability to use "regular" (non-private) browsing. In this post, I'll describe what I consider to be a better way of achieving this, at least on Linux.<!--more-->

It's possible this method will also work on Windows, but I haven't tested it. If anyone gets a chance to test it and let me know, I'll update this post and credit you accordingly. Just hit [me on Twitter][link-3] and let me know what you've found in your testing. I've also only tested this on [Fedora][link-5], but it should be the same or very similar for any distribution that uses GNOME.

GNOME uses the idea of "desktop files" (typically found in `/usr/share/applications` or `~/.local/share/applications`) to enable the launching of applications via the Activities screen or other mechanisms. (For more information on desktop files, see [here][link-4].) These desktop files specify where the executable is found, what command-line parameters to use, what icon to use, what name the application should go by, etc. Desktop files also allow application developers or users to define additional actions, such as opening a new window.

Firefox's desktop file is (at least on Fedora) found at `/usr/share/applications/firefox.desktop`. In that file, the `Exec` line in the `[Desktop Entry]` section instructs how to launch Firefox. Farther down, several actions are defined, one of which is opening a new private window. Each of these actions also has an `Exec` line. Looking at the `Exec` line for opening a private window versus the `Exec` line for opening a new window, you'll note that Firefox uses a `--private-window` parameter to control this behavior.

The trick here is to add `--private-window` to the `Exec` line in the `[Desktop Entry]` section of the desktop file, so that it looks like the `Exec` line in the section for opening a new private window. When you do this, launching Firefox will still open a "regular" browser window, but clicking on a link in any other application---e-mail, editor, terminal, whatever---will automatically open a new private browsing window. If a private browsing window is already open, it will open a new tab in that window.

So, to summarize:

1. Change the `/usr/share/applications/firefox.desktop` file to add `--private-window` to the command specified on the `Exec` line of the `[Desktop Entry]` section.
2. Firefox will still open a regular browser window when it is launched.
3. Links outside of Firefox will open a new private browsing window (or a new tab in an existing private browsing window).

The advantage of this approach versus some of the others is that you still have access to regular browser windows if/when they are needed. This configuration doesn't force private browsing all the time; rather, it just makes private browsing the default when opening links outside of Firefox. To me, that's a much more user-friendly experience than forcing private browsing for all sites.

One caveat to this approach is that your changes to the Firefox desktop file get overwritten any time `dnf update` installs an update for Firefox. I'm sure there's probably a workaround for this, but I haven't found it yet.

(By the way, the reason I say this _might_ work on Windows is because command-line parameters are exposed on Windows as well as on Linux through the use of shortcuts on the Start Menu. macOS does expose command-line parameters to a limited extent, but this functionality doesn't appear usable in any practical way.)

I hope this information is helpful to someone. Feel free to contact [me on Twitter][link-3] if you have any feedback, corrections, or suggestions for improvement.

[link-1]: https://ccm.net/faq/15012-how-to-start-firefox-in-private-mode-by-default
[link-2]: https://www.mozilla.org/en-US/firefox/products/
[link-3]: https://twitter.com/scott_lowe
[link-4]: https://specifications.freedesktop.org/desktop-entry-spec/desktop-entry-spec-latest.html
[link-5]: https://getfedora.org
