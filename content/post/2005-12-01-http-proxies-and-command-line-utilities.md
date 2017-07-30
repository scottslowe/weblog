---
author: slowe
categories: Information
comments: false
date: 2005-12-01T20:14:12Z
slug: http-proxies-and-command-line-utilities
tags:
- CLI
- Linux
- Networking
title: HTTP Proxies and Command-Line Utilities
url: /2005/12/01/http-proxies-and-command-line-utilities/
wordpress_id: 131
---

A few weeks ago, I locked down outbound HTTP traffic from my network. All outbound HTTP traffic must now pass through a caching HTTP proxy. (The proxy also performs some content filtering so that the kids don't access stuff they shouldn't.)

In my recent experiments with [Kubuntu](http://kubuntu.org/), I needed to download some small files using `wget`, but couldn't because of the proxy. As it turns out, there is a really simple fix.

Simply set an environment variable called `http_proxy` to the correct URL of the outbound proxy server, e.g., "http://proxy.company.com:8080" or whatever. In the bash shell, this can be easily accomplished with this command:

    export http_proxy="http://proxy.domain.com:8080"

Once this environment variable was set properly (I messed up a couple of times trying to set it, getting my `bash` syntax confused with my `tcsh` syntax), `wget` and `apt-get` worked like a charm. I haven't yet tried this with yum on Fedora Core 3, but I anticipate that it will work there as well.
