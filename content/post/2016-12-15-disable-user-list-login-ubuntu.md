---
author: slowe
categories: Tutorial
comments: true
date: 2016-12-15T00:00:00Z
tags:
- Linux
- CLI
- Ubuntu
title: Hiding the User List on the Ubuntu Login Screen
url: /2016/12/15/disable-user-list-login-ubuntu/
---

In this post, I'm going to share how to hide the user list on the login screen for [Ubuntu][link-1] 16.04. The information here isn't necessarily new or ground-breaking; however, in searching for the solution myself I found a lot of conflicting information as to how this may or may not be accomplished. I'm publishing this post in the hopes of providing a bit more clarity around this topic.

I've verified that this procedure works on the desktop distribution of Ubuntu 16.04. Note also that this is probably not the _only_ way of making this work; it's likely there are other ways of accomplishing the same thing.

To make configuration changes to the login screen, you'll want to add configuration files to `/etc/lightdm/lightdm.conf.d`. I used a single file to hide the user list and disable guest logins; presumably, you could use separate files for each configuration directive.

To disable the user list and disallow guest logins, add this content to a file in the `etc/lightdm/lightdm.conf.d` directory (I used the filename `00-hide-user-list.conf`):

``` text
[SeatDefaults]
greeter-hide-users=true
greeter-show-manual-login=true
allow-guest=false
```

Once this file is in place, you'll need to either restart your Ubuntu system, or restart the LightDM service. Note that restarting the LightDM service will kill your graphical applications, so be sure to save anything important first. The next time you log in, you should have to manually enter both the username and the password, and guest logins should be disabled.

Enjoy!

**UPDATE**: Via Twitter, I also found [this post by Matt Fischer][link-2], which has some outstanding information on configuring `lightdm.conf`. This is an excellent resource.



[link-1]: http://www.ubuntu.com/
[link-2]: http://www.mattfischer.com/blog/?p=343