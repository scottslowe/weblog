---
author: slowe
categories: Tutorial
comments: true
date: 2017-12-15T12:00:00Z
tags:
- Linux
- Fedora
- Productivity
title: Installing XMind 8 on Fedora 27
url: /2017/12/15/installing-xmind-8-on-fedora-27/
---

[XMind][link-2] is a well-known cross-platform mind mapping application. Installing the latest version of XMind (version 8) on Linux is, unfortunately, more complicated than it should be. In this post, I'll show how to get XMind 8 running on [Fedora][link-3] 27.<!--more-->

So why is installing XMind more complicated than it should be? For reasons unknown, the makers of XMind stopped using well-known Linux package mechanisms with this version of the software, providing only a ZIP archive to download and extract. (Previous versions at least provided a Debian package.) While the ZIP archive includes a very simplistic "setup script", the script does nothing more than install a few packages and install some fonts, and was written expressly for Debian-based systems. If you extract the archive and place the files outside of your home directory (as would be typical for installing an application on most desktop Linux distributions), you'll run into problems with permissions. Finally, the application itself is extraordinarily brittle with regards to file locations and such; it's easy to break it by simply moving the wrong file.

Through [some research][link-1] and some trial-and-error, I finally arrived at a configuration for XMind 8 on Fedora 27 that satisfies a couple criteria:

1. The application should reside outside the user's home directory in a location that is typical for third-party applications (for example, in the `/opt` directory).

2. All user-specific directories and information would reside in the user's home directory so as to eliminate the need for overly-permissive file/group permissions.

Here are the steps you can follow to get XMind 8 installed on Fedora 27 in a way that satisfies these criteria.

First, you'll need to install the "java-1.8.0-openjdk" package using `dnf install`. XMind has a few different prerequisite packages (lame, webkitgtk, and glibc), but in my tests on Fedora 27 system this was the only package that wasn't already installed. Note that if you're trying to replicate these instructions on a different Linux distribution, this step is where you'll need to locate distribution-specific package names (most of the rest of the steps are applicable to any Linux distribution).

Next, download the XMind 8 ZIP archive, and extract it into an `xmind` directory:

    unzip xmind-8-update6-linux.zip -d xmind

For simplicity's sake, I chose to work within my own `~/Downloads` directory, but you should be able to work from within any directory where you have write permissions.

Third, create a user-specific area for XMind to store information:

    mkdir -p ~/.config/xmind/workspace

I chose the `~/.config` directory since it was already present and utilized for application-specific information. You can use a different directory, but the rest of the instructions will assume this path was used.

Fourth, go ahead and remove the 32-bit version of the XMind executable; it's pretty likely you won't need it:

    rm -rf xmind/XMind_i386

Fifth, copy over two directories into the user-specific area you created earlier:

    cp -a xmind/XMind_amd64/configuration ~/.config/xmind/
    cp -a xmind/XMind_amd64/p2 ~/.config/xmind/

Based on my testing, this step should be the only step needed to make XMind work for additional users on your Fedora system (i.e., you'll want to run this step for other users on the system as well in order for them to be able to run XMind).

Next, you'll need to update `XMind.ini` to tell XMind the new user-specific locations. Edit this file (found in the `XMind_amd64` subdirectory), and make the following changes:

* On line 2, change `./configuration` to `@user.home/.config/xmind/configuration` (the `@user.home` refers to the user's home directory; note that you _can't_ use the tilde shortcut here as it won't work)
* On line 4, change `../workspace` to `@user.home/.config/xmind/workspace`

You're almost done! In the `fonts` subdirectory of the `xmind` directory in which you stored the extracted files, you'll find some fonts that are distributed with XMind. Install these on your system (copy them to `~/.local/share/fonts` or `/usr/share/fonts`, as you desire), and then remove the `fonts` subdirectory. In my particular case, some of the fonts---like the Roboto family---were already installed.

Finally, move the `xmind` directory to its final location and lock down the permissions:

    sudo mv xmind /opt/
    sudo chown -R root:root /opt/xmind

The last and final step is to create a desktop entry file so that XMind is accessible via the launcher. Here's a sample file:

```
[Desktop Entry]
Comment=Create and share mind maps
Exec=/opt/xmind/XMind_amd64/XMind %F
Path=/opt/xmind/XMind_amd64
Name=XMind
Terminal=false
Type=Application
Categories=Office;Productivity
Icon=xmind
```

This desktop file can go into `/usr/share/applications` or `~/.local/share/applications`, though---for reasons I'll share shortly---the latter may be a better choice.

That's it! You should be able to launch XMind from the GNOME Activities screen.

Unfortunately, there are some caveats and limitations:

* XMind apparently stores its application icon buried deep in the `~/.config/xmind/configuration` directory structure. This is why you may prefer to use `~/.local/share/applications` for the desktop file. If you do use this location, you'll need to perform this step for other users of the system as well. (The `icon=xmind` line in the sample desktop file above won't actually work.)
* This manual installation doesn't create a MIME type for XMind documents so that you can open an XMind document from within the Nautilus file manager. (The simplistic shell script supplied in the XMind download doesn't either.) I've done a bit of work on this, but haven't come to a workable solution yet.

So there you have it: how to get XMind 8 installed and running in some reasonable fashion on Fedora 27. If you have questions, comments, or corrections, feel free to [hit me up on Twitter][link-4].



[link-1]: https://www.xmind.net/m/PuDC
[link-2]: https://www.xmind.net/
[link-3]: https://getfedora.org/
[link-4]: https://twitter.com/scott_lowe
