---
author: slowe
categories: Education
comments: true
date: 2021-06-22T08:30:00-06:00
tags:
- CLI
- Encryption
- macOS
- VPN
title: Making WireGuard from Homebrew Work on an M1 Mac
url: /2021/06/22/making-wireguard-from-homebrew-work-on-an-m1-mac/
---

After writing [the post on using WireGuard on macOS][xref-1] (using the official [WireGuard][link-1] GUI app from the Mac App Store), I found the GUI app's behavior to be less than ideal. For example, tunnels marked as on-demand would later show up as no longer configured as an on-demand tunnel. When I decided to set up WireGuard on my M1-based MacBook Pro (see [my review][xref-2] of the M1 MacBook Pro), I didn't want to use the GUI app. Fortunately, [Homebrew][link-2] has formulas for WireGuard. Unfortunately, the WireGuard tools as installed by Homebrew on an M1-based Mac won't work. Here's how to fix that.<!--more-->

The key issues with WireGuard as installed by Homebrew on an M1-based Mac are:

* On an M1-based Mac, Homebrew installs (by default) to the `/opt/homebrew` prefix. By comparison, Homebrew uses `/usr/local` on Intel-based Macs. Some of the WireGuard-related scripts are hard-coded to use `/usr/local` as the Homebrew prefix. Because the prefix has changed, though, these scripts now don't work on an M1-based Mac.
* WireGuard has a dependency on [Bash][link-3]. Unfortunately, the version of Bash supplied by macOS isn't supported by WireGuard (it's too old). Without a very specific PATH configuration, even installing the Homebrew version of Bash---which _is_ supported by WireGuard---won't help. (You'll have to install it anyway; Homebrew requires it to satisfy a dependency.)

Fortunately, there is a way to work around these issues. The following steps should enable you to use Homebrew's WireGuard on your M1-based Mac:

1. Go ahead and use `brew install` to install the "wireguard-go", "wireguard-tools", and "bash" packages.
2. Edit your shell startup file(s) to modify your PATH variable in such a way so that a personal directory---like `$HOME/bin` or similar---is listed first, followed by the `/opt/homebrew/bin` directory. (I _think_ the order of the `/opt/homebrew/bin` directory in the PATH may not matter, but I need to do more testing before I'll know for certain.)
3. Copy the `wg-quick` script installed by Homebrew into the personal directory specified in the previous step. This is necessary because the script is read-only in the location where Homebrew installs it.
4. Edit the shebang line of the `wg-quick` script to say `#!/usr/bin/env -P/opt/homebrew/bin bash`. Basically, you're adding `-P/opt/homebrew/bin` to the shebang line, right before `bash`. (You can check `man env` to understand what the `-P` parameter does, but the "TL;DR" is that you're limiting where `env` will search to find a `bash` binary.)
5. On or about line 44, the script defines a variable named `CONFIG_SEARCH_PATHS`. Edit this line to add `/opt/homebrew/etc/wireguard` to the existing list of directories that the script will search for the WireGuard configuration files.

Once you've done these steps, you can proceed with configuring WireGuard as outlined in a variety of articles and posts (such as this one I wrote on [configuring WireGuard on macOS via the CLI][xref-3]). Replace any references to `/usr/local/etc/wireguard` in these posts with `/opt/homebrew/etc/wireguard`.

When you run `sudo wg-quick up wg0` to activate the VPN, your shell will find your modified version of this script first (because of step 2 above). This means your changes to the script will be in effect. Your modified version of this script will find the Homebrew-installed version of Bash (because of the change made in step 4, limiting where `env` will look to find the `bash` binary), which is compatible with WireGuard and will allow the script to complete properly. Finally, because you added the `/opt/homebrew/etc/wireguard` directory to the list of paths that `wg-quick` will search for WireGuard configuration files, it will find your configuration files and appropriately configure the interface(s).

The steps described above should get you a working WireGuard installation on an M1-based Mac. Hopefully, I can contribute these changes (or improved versions of these changes) back to the Homebrew project. If you have questions, or if I have made a mistake in my recommendations, please contact [me on Twitter][link-4]. I hope you find this useful!

[link-1]: https://www.wireguard.com
[link-2]: https://brew.sh
[link-3]: https://www.gnu.org/software/bash/
[link-4]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2021-04-01-using-wireguard-on-macos.md" >}}
[xref-2]: {{< relref "2021-06-02-review-2020-m1-based-macbook-pro.md" >}}
[xref-3]: {{< relref "2021-06-28-using-wireguard-on-mac-via-cli.md" >}}
