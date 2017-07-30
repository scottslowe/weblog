---
author: slowe
categories: News
comments: true
date: 2008-02-25T12:24:04Z
slug: vi-plugins-expose-a-problem
tags:
- Security
- Virtualization
- VMware
title: VI Plugins Expose a Problem
url: /2008/02/25/vi-plugins-expose-a-problem/
wordpress_id: 642
---

In his work developing VI plugins, Andrew Kutz discovered a potential security flaw. Quoting from the e-mail he sent me:

>It troubles me that the VI client will allow any plugin to be loaded regardless of whether or not it may contain dangerous code. During the development of the Console plugin I had to register a message filter on the primary message loop to capture input for the SSH "terminal." I was not sure if the VI client would allow me to do this, as the ability to so has nasty implications. Well, it does, and it does.  
>
>Enter my KeySniffer plugin. KeySniffer? is an example of how VI 2.5 client plugins can be abused. This plugin sniffs all key strokes that occur within the VI 2.5 client and outputs them to `C:\viclientkeystrokes.txt`. KeySniffer works by registering a message filter on the VI client's message loop and recording all input to aforementioned file.

Andrew's advice? Be sure that you **ABSOLUTELY** trust the source of any plugins that you may load inside the VI Client. Otherwise, you could be exposing your information in a dangerous and unexpected way.

Andrew has also provided an executable that can check for this problem as well.

Thanks for pointing this out, Andrew.
