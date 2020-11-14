---
author: slowe
categories: Tutorial
comments: true
date: 2012-04-24T09:00:00Z
slug: running-synergy-server-automatically-on-os-x-lion
tags:
- macOS
- OSS
title: Running Synergy Server Automatically on OS X Lion
url: /2012/04/24/running-synergy-server-automatically-on-os-x-lion/
wordpress_id: 2596
---

In November of last year, I wrote [this article][1] on how I was using [Synergy](http://synergy-foss.org/) to share a single keyboard and mouse across two Mac laptops and an Ubuntu Linux laptop. At that time, my 13" MacBook Pro was the Synergy server and used [ControlPlane](http://www.controlplaneapp.com/) to automatically start or stop the Synergy server process. ControlPlane looked at whether the laptop was connected to the 24" LED Cinema Display (which meant I was in my home office); if so, it launched the Synergy server. When it was disconnected from the display, it killed the Synergy server. For the most part, this setup worked reasonably well.

However, I recently acquired a new dual quad-core Mac Pro workstation, and as part of the setup for the new Mac Pro I wanted it to be the new Synergy server. This would make things easier; the 13" MacBook Pro would then only need to know whether or not it needed to run the Synergy client, and the Synergy server process could simply run full-time on the Mac Pro. However, in order to do that, I needed to configure the Mac Pro (running OS X Lion 10.7.3) to run `synergys` automatically. Here's how I did it.

First, I wrote a simple shell script that checks for the presence of `synergys` already running. If `synergys` is already running, do nothing; otherwise, start `synergys`. Here's the shell script:

```sh
#!/bin/sh
# Startup script for the Synergy server component

# Check for synergys running
number=$(ps ax | grep "[/]synergys" | wc -l)

# Start synergys in foreground if not already running
if [ $number -gt 0 ]
  then
  	echo Running
  else
    /usr/bin/synergys -f -c /etc/synergy.conf
fi
```

Based on the testing I've done so far, this script works well. If you're a shell scripting expert and have a better way of handling this, let me know (I'm always open to suggestions for improvement).

The next step was to configure `launchd` to automatically run this shell script---and therefore run `synergys`---every time I booted the Mac Pro. To do this, I created a property list file, also known as a plist file, in the `/Library/LaunchAgents` directory. I named the file `org.synergy-foss.synergys.plist`, but you can use a different name if you wish.

Here's the contents of the plist file I created:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE plist PUBLIC "-//Apple Computer/DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
<key>Label</key>
<string>org.synergy-foss.synergys</string>
<key>ProgramArguments</key>
<array>
<string>/usr/local/bin/synlaunch.sh</string>
</array>
<key>RunAtLoad</key>
<true/>
<key>ServiceDescription</key>
<string>Synergy server daemon</string>
</dict>
</plist>
```

Of course, `synergys` also needs its configuration file. You can read about the configuration file I used in [my original Synergy article][1]. I modified it slightly to remove the double-tap feature and to add corners where the cursor wouldn't switch screens (to make it easier to get to the Apple and Spotlight menus).

With the configuration file, shell script, and property list file in place, I rebooted the Mac Pro. Once the Mac Pro was booted, a `netstat -tan | grep LISTEN` showed that `synergys` was listening on the default port of TCP 24800. I then launched `synergyc` on the 13" MacBook Pro and it connected flawlessly. Continued testing did not show any problems. The only final step was to reconfigure ControlPlane to start or stop `synergyc`--the client portion of Synergy---in response to whether the laptop was docked.

So, if you're in a need to have the Synergy server run automatically on your Mac OS X system, this information should get you up and running. If you have any questions, clarifications, or suggestions for improvements, I encourage you to speak up the comments below.

[1]: {{< relref "2011-11-18-some-oss-with-my-mac-part-1-synergy.md" >}}
