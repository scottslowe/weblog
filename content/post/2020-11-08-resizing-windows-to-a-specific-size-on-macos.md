---
author: slowe
categories: Information
comments: true
date: 2020-11-08T16:00:00-07:00
tags:
- macOS
- AppleScript
title: Resizing Windows to a Specific Size on macOS
url: /2020/11/08/resizing-windows-to-a-specific-size-on-macos/
---

I recently had a need (OK, maybe more a desire than a need) to set my browser window(s) on macOS to a specific size, like 1920x1080. I initially started looking at one of the many macOS window managers, but after reading lots of reviews and descriptions and still being unclear if any of these products did what I wanted, I decided to step back to using AppleScript to accomplish what I was seeking. In this post, I'll share the solution (and the articles that helped me arrive at the solution).<!--more-->

My first stop was [this blog post][link-2] by Ethan Banks. I tried replicating the AppleScript he used, but couldn't get it to work. I'm still running macOS 10.14 "Mojave," so perhaps his code was specific to macOS 10.15 "Catalina." I moved on, never realizing there was another section to his post that had the information I needed (and would eventually find). Let that be a lesson to be sure to read the _entire_ post next time.

Moving on, I arrived at [this post][link-3]. OK, this used a different mechanism than Ethan's post. I tried it, and it _sort of_ worked, but it didn't create the window geometry I was expecting. As I'll later learn, it was just due to an incomplete understanding on my part of how the `set bounds` command works in AppleScript.

Finally, I found [this article][link-1] that shared how to use AppleScript in conjunction with Automator to create a macOS Service to resize the current window of the active application. After trying it for a while, and not getting the results I wanted, I started digging again to see what it was that I was doing wrong.

I found the answer to what I was doing wrong [here][link-4]. The parameters to the `set bounds` command had been illustrated as `x-position, y-position, width, height`, but they should be more _accurately_ described as `starting-x-position, starting-y-position, ending-x-position, ending-y-position`. My mistake was that I was providing the desired window size (like 1920x1080 or 1280x720) as the last two parameters, when what I needed to be providing was the desired window size **plus** the starting X and Y position, respectively. So, if the window was placed 300 pixels away from the left edge and I wanted to the window to be 1920 pixels wide, then the third parameter needed to be 2220 (300 + 1920 = 2220). Ah! I had seen one of the examples doing this but didn't understand _why_, so I hadn't included the portion of the code. Once I fixed my code accordingly, then it started working exactly as expected.

(This piece of missing information, by the way, is also found at the bottom of Ethan's post---the one I started with. Go figure!)

Nothing earth-shattering here, I know, but I wanted to share it nevertheless just in case it would benefit others. Contact [me on Twitter][link-5] if you have any questions.

[link-1]: https://www.elevatecreates.com/automatically-resize-mac-application-window/
[link-2]: https://ethancbanks.com/2020/02/18/using-applescript-to-size-a-window-to-16x9-on-macos/
[link-3]: https://alvinalexander.com/source-code/mac-os-x/how-size-or-resize-application-windows-applescript/
[link-4]: http://macosxautomation.com/applescript/firsttutorial/11.html
[link-5]: https://twitter.com/scott_lowe
