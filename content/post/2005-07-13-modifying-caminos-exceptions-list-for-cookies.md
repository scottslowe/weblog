---
author: slowe
categories: Explanation
comments: false
date: 2005-07-13T22:36:41Z
slug: modifying-caminos-exceptions-list-for-cookies
tags:
- macOS
- Web
title: Modifying Camino's Exceptions List for Cookies
url: /2005/07/13/modifying-caminos-exceptions-list-for-cookies/
wordpress_id: 48
---

I'm a stickler about cookies. No, not the kind you eat...the kind that web servers place on your computer when you visit a web site. I understand that cookies are sometimes necessary to maintain state as a user navigates through a web site; HTTP is, after all, a stateless protocol. But a cookie for every site no matter what? That just gets on my nerves.

Up until now, I had [Camino](http://www.caminobrowser.org/) configured to ask if I wanted to allow a cookie or not. This got time-consuming when I was visiting a site I had never visited before and all the ad servers wanted to place a cookie to track me. I wanted a way to tell Camino, "Block all cookies unless I say otherwise." Finally, in Camino 0.9a1, I can set all cookies to be denied and then view the exception list, but there was still no way to edit the exception list.

Being the determined (read: stubborn) person that I am, I was determined to find a way to edit this exceptions list. (As a side note to the Camino developers, please write a UI for editing the exception list.) After not too much searching, I found that the `hostperm.1` file in `~/Library/Application Support/Camino` is an ordinary text file that contains the exceptions list. To add a site to this list, simply follow the same format as one of the existing lines, specifying 1 to allow cookies or 2 to deny cookies. Of course, any editing to this file should be done while Camino is not running.
