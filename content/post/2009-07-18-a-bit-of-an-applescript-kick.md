---
author: slowe
categories: Information
comments: true
date: 2009-07-18T23:46:58Z
slug: a-bit-of-an-applescript-kick
tags:
- Automation
- macOS
- Productivity
title: A Bit of an AppleScript Kick
url: /2009/07/18/a-bit-of-an-applescript-kick/
wordpress_id: 1467
---

I've been on a bit of an AppleScript kick recently. I'm not 100% sure exactly why, but I have. I've always been a bit of a fan of Apple's "natural language" scripting engine, but I've also always been frustrated that more applications don't support it. Even Apple's own applications don't support AppleScript as well as some other applications do.

In the event that you like using AppleScript, too, I thought I'd post my scripts here, along with a brief description of each one. You may need to modify them to suit your needs, and I'll open deny any liability for anything that happens if you decide to use them. If you suddenly become more productive, it's not my fault.

These scripts were written for use by/with the following applications, and all scripts (where applicable) offer support for notifications via [Growl](http://growl.info):

* [OmniFocus](http://www.omnigroup.com/applications/omnifocus/)  
* [NetNewsWire](http://www.newsgator.com/INDIVIDUALS/NETNEWSWIRE/)  
* [Twitterrific](http://iconfactory.com/software/twitterrific)  
* [Camino 2.0 Beta 3](http://preview.caminobrowser.org/)  
* [Safari 4.0.2](http://www.apple.com/safari/)

I wrote a couple of AppleScripts for Viscosity as well, but those are so ridiculously simple that I won't even bother posting them. Just look at [this link](http://www.viscosityvpn.com/support/?section=faq&supportid=5) and you'll see the extent of Viscosity's AppleScript support.

Without any further delay, then, here are the scripts!

## Send Headline to OmniFocus

_[Download Link](/public/dl/nnw-to-of.zip)_

I think I got the basis for this script somewhere else, but honestly I can't remember where. The idea behind this script is that when you see a headline in NetNewsWire that looks interesting, you invoke this script and it creates an action in OmniFocus for you. That way, you can quickly scan all the headlines in NetNewsWire, create OmniFocus actions for those that warrant further attention, and then get back to whatever it was you were doing previously. Like most---if not all---of the scripts here, I generally invoke this script using Quicksilver.

## Send Tweet to OmniFocus

_[Download Link](/public/dl/tweet-to-of.zip)_

This script was directly based on [this blog entry](http://andy.theschotts.net/2009/01/sending-current-twitterrific-tweet-to-omnifocus.html), so I'm not due any of the credit. Once again, the idea is to support quick information aggregation. You see a tweet in Twitterrific that looks interesting, or perhaps it has a link in it that you want to investigate further. Invoke the script and it will add an action to OmniFocus with the contents of the tweet. Later, when it fits into your workflow, you can perform a more in-depth review of the information and process it appropriately.

## Retweet Unmodified Post

_[Download Link](/public/dl/retweet-twitterrific.zip)_

The current version of Twitterrific (version 3.2) lacks quite a bit of functionality, like retweeting, view the whole conversation, etc. So, to work around the lack of built-in retweet functionality, I wrote this script. It's not the greatest in the world, but it helps a little bit. Select the tweet you want to retweet, invoke the script, and your Mac takes care of the rest.

## Send URL to OmniFocus (Camino)

_[Download Link](/public/dl/camino-to-of.zip)_

You can probably guess the purpose behind this script: to take the information from the frontmost Camino window and create a task in OmniFocus. Yes, that's right, I have very little imagination. I figure if you limit the amount of browsing you do to defined times (something I'm still working to perfect) then you can instead focus on more important things---you know, like getting your work done. Bosses like that. You only need to invoke the script and it will automatically pull the URL and the page title and push them into OmniFocus.

I also have [a Safari version](/public/dl/safari-to-of.zip).

## Send URL to Pukka (Camino)

_[Download Link](/public/dl/camino-to-pukka.zip)_

After mourning the loss of Cocoalicious for quite some time, I switched to Pukka. Pukka offers a built-in service accessible via the Services menu, but it wasn't picking up enough information from the browser. So I wrote this script to retrieve the URL, page title, and selected text (if any) from the frontmost Camino window and feed them to Pukka. The user then adds any tags and posts the URL to Delicious.com.

## Open Source in TextMate (Camino)

_[Download Link](/public/dl/camino-to-tm.zip)_

This one was more challenging than I had anticipated. I suspected TextMate's AppleScript support would be more than it was, which was---quite frankly---rather disappointing. In any case, invoke this script and the HTML source from the frontmost Camino browser window will be opened in TextMate.

Well, that's it. If you have any useful AppleScript scripts, tips, tricks, or other trivia to share, please speak up in the comments.
