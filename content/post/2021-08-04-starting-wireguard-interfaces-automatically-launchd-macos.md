---
author: slowe
categories: Education
comments: true
date: 2021-08-04T06:00:00-06:00
tags:
- CLI
- macOS
- VPN
title: Starting WireGuard Interfaces Automatically with Launchd on macOS
url: /2021/08/04/starting-wireguard-interfaces-automatically-launchd-macos
---

In late June of this year, I wrote a piece on [using WireGuard on macOS via the CLI][xref-1], where I walked readers using macOS through how to configure and use the [WireGuard VPN][link-1] from the terminal (as opposed to using the GUI client, which I discussed [here][xref-2]). In that post, I briefly mentioned that I was planning to explore how to have macOS' `launchd` automatically start WireGuard interfaces. In this post, I'll show you how to do exactly that.<!--more-->

These instructions borrow heavily from [this post showing how to use macOS as a WireGuard VPN server][link-2]. These instructions also assume that you've already walked through installing the necessary WireGuard components, and that you've already created the configuration file(s) for your WireGuard interface(s). Finally, I wrote this using my M1-based MacBook Pro, so my example files and instructions will be referencing the default [Homebrew][link-3] prefix of `/opt/homebrew`. If you're on an Intel-based Mac, change this to `/usr/local` instead.

The first step is to create a `launchd` job definition. This file should be named `<label>.plist`, and it will need to be placed in a specific location. The `<label>` value is taken from the name given to the job itself, which you'll see in the example job definition below. Since this modifies the networking configuration of your macOS system, it will need to be treated as a "global daemon" and will need to be placed in the `/Library/LaunchDaemons` directory.

Here's an example of a job definition:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>
        <key>Label</key>
        <string>com.wireguard.wg0</string>
        <key>ProgramArguments</key>
        <array>
            <!-- Points to local version of wg-quick that
                 fixes path issues with the script -->
            <string>/Users/slowe/.local/bin/wg-quick</string>
            <string>up</string>
            <string>wg0</string>
        </array>
        <key>KeepAlive</key>
            <dict>
                <key>NetworkState</key>
                <true/>
            </dict>
        <key>RunAtLoad</key>
        <true/>
        <key>StandardErrorPath</key>
        <string>/opt/homebrew/var/log/wireguard.err</string>
        <key>EnvironmentVariables</key>
        <dict>
            <key>PATH</key>
            <!-- Adds in user-specific and Homebrew bin directories to start of PATH -->
            <string>/Users/slowe/.local/bin:/opt/homebrew/bin:/usr/local/sbin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin</string>
        </dict>
    </dict>
</plist>
```

For the most part, the job definition is pretty easy to figure out, but here's a few notes:

* The `<label>` reference in naming the file pertains to the value of the `Label` key in the job definition. In my example, I'm calling the job "com.wireguard.wg0", so the filename should be `com.wireguard.wg0.plist`. Note that the extension is necessary.
* Note in the `ProgramArguments` key that I'm referencing my personal changed copy of the Homebrew `wg-quick` script, which works around some path- and shell-related issues on M1-based Macs (as described [here][xref-3]).
* As mentioned earlier, users of Intel-based Macs would want to change references to `/opt/homebrew` over to `/usr/local` (and the reference to the local copy of `wg-quick` may not be necessary).

Make sure this job definition is saved to an appropriate file name (`<label>.plist`) and it's found in the `/Library/LaunchDaemons` directory. Then run these commands to inform `launchd` of the new job definition you just created:

    sudo launchctl enable system/com.wireguard.wg0
    sudo launchctl bootstrap system /Library/LaunchDaemons/com.wireguard.wg0.plist

After running these two commands, you should be able to run `wg show` and see your WireGuard interface up and running. It should get turned up every time you restart your computer and there is a network present/active.

## Additional Resources

In addition to [the Barrowclift post][link-2], I also found the [launchd.info][link-4] site to be quite helpful.

Although the above instructions work without any issues on my Mac, I can't guarantee they'll work on every system out there. If you run into issues, or if you find that I've provided incorrect or incomplete information, please let me know. You can interact with [me on Twitter][link-5], or drop me an e-mail (my address isn't too hard to find).

[link-1]: https://www.wireguard.com/
[link-2]: https://barrowclift.me/post/wireguard-server-on-macos
[link-3]: https://brew.sh
[link-4]: https://launchd.info/
[link-5]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2021-06-28-using-wireguard-on-mac-via-cli.md" >}}
[xref-2]: {{< relref "2021-04-01-using-wireguard-on-macos.md" >}}
[xref-3]: {{< relref "2021-06-22-making-wireguard-from-homebrew-work-on-an-m1-mac.md" >}}
