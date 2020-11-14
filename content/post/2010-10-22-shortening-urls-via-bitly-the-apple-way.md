---
author: slowe
categories: Tutorial
comments: true
date: 2010-10-22T11:18:07Z
slug: shortening-urls-via-bitly-the-apple-way
tags:
- Automation
- macOS
- Web
title: Shortening URLs via bit.ly the Apple Way
url: /2010/10/22/shortening-urls-via-bitly-the-apple-way/
wordpress_id: 2139
---

A couple of days ago I wrote about how to use the UNIX CLI in Mac OS X [to shorten a URL via bit.ly][1], while adding the URL to your link history in case you want to re-use it in the future. Now I'm going to take that information and show you how to further integrate this into your Mac's environment using AppleScript and Automator.

The necessary glue here are these two facts:

1. AppleScript can execute a shell script using `do shell script`; this is what allows us to leverage the `curl` command I discussed in the previous post from within AppleScript.

2. Automator can execute AppleScripts via the Run AppleScript action. This allows us to take the AppleScript (which is executing the shell script) and embed it into an Automator workflow.

To give credit where credit is due, this isn't my idea at all; I've derived all this information from [this post by David Poindexter](http://davidrpoindexter.com/tutorial/bit-ly-url-shortening-with-mac-os-x-snow-leopard-services-and-applescript/). His shell command is different and doesn't populate the user's link history, but it does work. Robert Huttinger also built [his own workflow](http://www.apple.com/downloads/macosx/automator/bitlyurlconverter_roberthuttinger.html), which served as a basis for my own.

First, here's the AppleScript code that wraps around the `curl` command to shorten the URL:

```text
on run (input)  
	set login to "YourUserNameHere" as string  
	set apiKey to "YourAPIKeyHere" as string  
	set input to (input as string)  
	ignoring case  
	if (((characters 1 thru 4 of input) as string) is not equal to "http") then  
		beep  
		return  
	else  
		set curlCmd to "curl --stderr /dev/null \"http://api.bit.ly/v3/shorten?login=" & login & "&apiKey=" & apiKey & "&longURL=" & input & "&format=txt\""  
		set shortURL to (do shell script curlCmd)  
		return shortURL  
	end if  
	end ignoring  
end run
```

Be careful with the line starting "set curlCmd..."; it's wrapped above and you'll need to properly escape the quotes with backslashes, as above, in order for it to work properly. You'll clearly want to replace "YourUserNameHere" and "YourAPIKeyHere" with the appropriate values from your bit.ly account.

A text version of the script is available for download [here](/public/dl/bitly-script.txt).

Once you have the AppleScript written, you can then embed it into an Automator workflow. I won't bother to explain what Automator is or how it works here; there are numerous resources available to help in that regard. Rather, I'll just simply say that you only need to assemble the Run AppleScript, (optionally) the Show Growl Notification, and the Copy to Clipboard actions as shown in [this screenshot](/public/img/bitly-workflow.png). In my case, I'm using Automator to create a service that accepts text from any application; this means I need only select the text of a URL I'd like to have shorten and then invoke this service. After a brief pause, a [Growl](http://growl.info) notification pops up and the shortened bit.ly URL is on my clipboard, ready to be pasted into whatever application I need. And, since it's now a Mac OS X Service, you can bind it to a hotkey for even easier access.

Again, credit goes to the others who have blazed this trail ahead of me; I'm merely posted my version here in the event it is useful to others. Comments, feedback, and suggestions are always welcome.

[1]: {{< relref "2010-10-20-shortening-urls-via-bitly-from-the-cli.md" >}}
