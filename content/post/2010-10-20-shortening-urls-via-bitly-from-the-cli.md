---
author: slowe
categories: Information
comments: true
date: 2010-10-20T18:33:39Z
slug: shortening-urls-via-bitly-from-the-cli
tags:
- CLI
- macOS
- Web
- UNIX
title: Shortening URLs via bit.ly from the CLI
url: /2010/10/20/shortening-urls-via-bitly-from-the-cli/
wordpress_id: 2135
---

I was experimenting tonight with some ways to add more automation to my workflow. One process that is (relatively) time-consuming is the process of generating short URLs via [bit.ly](http://bit.ly). This site had [a brief tutorial](http://nnutter.com/2009/03/automate-bitly-with-applescript/) on how to use `curl` to do it, but the shortened link didn't show up in my link history. Upon browsing the [bit.ly API documentation](http://code.google.com/p/bitly-api/wiki/ApiDocumentation), though, I was able to fairly quickly piece together a command line that will shorten a URL via bit.ly _**and**_ put the shortened URL in the user's link history.

Note that in order to use this command, you'll need your bit.ly API key. Your API key is easily accessed from your account settings page.

Here's the command I tested (works on Mac OS X 10.6.4):

	curl 'http://api.bit.ly/v3/shorten?login=<bit.ly login>&apiKey=<bit.ly API key>&longURL=<Long URL to be shortened>&format=txt'

Note you'll need to supply appropriate values for `<bit.ly login>`, `<bit.ly API key>`, and `<Long URL to be shortened>`.

In order to make this truly usable, there are some additional things that have to happen. The long URL has to be properly encoded, as it can't have any spaces or special characters, for example. But otherwise, this command is a workable solution to shortening a URL from the command line. All I need now is a small AppleScript around this and then I'll have a URL shortening script I can bind to a hotkey. That should help speed the process up!
