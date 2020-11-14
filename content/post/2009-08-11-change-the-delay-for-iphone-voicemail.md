---
author: slowe
categories: Tutorial
comments: true
date: 2009-08-11T10:44:00Z
slug: change-the-delay-for-iphone-voicemail
tags:
- iPhone
- iOS
title: Change the Delay for iPhone Voicemail
url: /2009/08/11/change-the-delay-for-iphone-voicemail/
wordpress_id: 1515
---

For a couple of different reasons---one of them being a desire to have Google Voice take over voicemail responsibilities---I needed to find a way to make my iPhone wait longer before sending an unanswered call to voicemail. As it turns out, [this thread](http://www.google.com/support/forum/p/voice/thread?tid=06b28869a293df1b&hl=en) has the answer I needed. Here's how it works. _(Note: Do this at your own risk. I'm not responsible for any problems you might create as a result of this information.)_

First, you'll want to get the number to which your calls are transferred when they are transferred to voicemail. Just dial `*#61#` and dial it. Your iPhone screen will darken and then some text will appear that says something to the effect of "Service Interrogation Complete". The voicemail number will be listed there. Write it down.

Next, you'll dial another sequence to tell the iPhone to wait 30 seconds (the maximum delay that can be set) before transferring unanswered calls to voicemail. Dial `*61*16787641234*11*30#`, replacing the 11 digits between `*61*` and `*11*30#` with the voicemail number you wrote down in the previous step. When you dial this number, the iPhone screen will darken again and tell you that the setting was complete. Note that it won't tell you the delay that was set.

Now you'll have more time to get that iPhone out of your pocket, purse, or backpack before you miss the call. Or, as in my case, the iPhone delay will cause Google Voice to pick up the voicemail first. Enjoy!
