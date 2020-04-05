---
author: slowe
categories: Tutorial
comments: true
date: 2018-05-15T12:00:00Z
tags:
- Linux
- Fedora
- Web
- CLI
- Firefox
title: Manually Installing Firefox 60 on Fedora 27
url: /2018/05/15/manually-installing-firefox-60-on-fedora-27/
---

Mozilla recently released version 60 of [Firefox][link-1], which contains a number of pretty important enhancements (as outlined [here][link-3]). However, the [Fedora][link-2] repositories don't (yet) contain Firefox 60 (at least not for Fedora 27), so you can't just do a `dnf update` to get the latest release. With that in mind, here are some instructions for manually installing Firefox 60 on Fedora 27.<!--more-->

These instructions assume you have a `dnf`-installed version of Firefox (typically Firefox 59) already installed on your Fedora system. These steps should allow you to upgrade your Fedora system to Firefox 60:

1. Download the Firefox 60 archive (typically named `firefox-60.0.tar.bz2` or similar) onto your Fedora system. You can do this with your already-installed version of Firefox, but be sure to close/quit Firefox before proceeding with the rest of the instructions.
2. Make a copy of `/usr/share/applications/firefox.desktop`; you'll use this later.
3. Remove the version of Firefox installed from the Fedora repositories with `dnf remove firefox`. This will remove the `firefox.desktop` file you copied in the previous step (which is why you copied it somewhere else).
4. Use `bunzip2` to decompress the downloaded Firefox 60 archive. This will leave you with a plain `.tar` file:

        bunzip2 firefox-60.0.tar.bz2

5. Extract the archive into your `/opt` directory:

        sudo tar -C /opt -xvf firefox-60.0.tar

    This will create a `firefox` directory underneath the `/opt` directory. (You can modify this as desired to install Firefox 60 into a different location.)
6. Edit the saved copy of `firefox.desktop` (from step 2) and replace the "Exec=" lines with the correct path to the new Firefox 60 binary (if you're following these instructions, the correct path will be `/opt/firefox/firefox`).
7. Copy the edited `firefox.desktop` file (from the previous step) back into `/usr/share/applications`.

And that's it! You should now be good to go with Firefox 60 on your Fedora 27 system. If you run into any problems or have any additional questions, feel free to [hit me up on Twitter][link-4].

[link-1]: https://www.mozilla.org/en-US/firefox/
[link-2]: https://getfedora.org/
[link-3]: https://www.mozilla.org/en-US/firefox/60.0/releasenotes/
[link-4]: https://twitter.com/scott_lowe
