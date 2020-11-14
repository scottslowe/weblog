---
author: slowe
categories: Review
comments: true
date: 2011-10-25T23:30:22Z
slug: my-experiences-with-lion
tags:
- macOS
- Apple
title: My Experiences with Lion
url: /2011/10/25/my-experiences-with-lion/
wordpress_id: 2450
---

About two weeks ago, I purchased a new 13" MacBook Pro for use as my primary laptop. As you probably know, I've been a Mac user for a while; this 13" MBP replaces a 15" MBP that I purchased about two years ago. Predictably, the new laptop was preloaded with Mac OS X 10.7 "Lion", the latest and greatest OS from Apple. Since the release of Lion, I'd made no secret of the fact that I had no intention of upgrading my existing laptop from 10.6 to 10.7, as I didn't see any real need to upgrade---Mac OS X 10.6 (aka "Snow Leopard") was working just fine for me. If the existing OS is running just fine and you don't need any of the features of the new OS, what's the point in upgrading?

Since the new laptop came preloaded with Lion, I decided that I should at least give the new OS an honest try and see if I liked it. With that in mind, I upgraded the new laptop to the latest Lion release, 10.7.2, and installed any and all applicable software updates. I then set out to migrate all my files, apps, and data, which I managed to complete just before leaving for Copenhagen for VMworld EMEA 2011.

Throughout the show and the accompanying travel, the new 13" MacBook Pro was my primary system, and I used it for all my daily tasks: e-mail, browsing the web, posting to Twitter, writing documents, creating presentations, building mind maps, configuring network equipmentyou get the idea. Here are some of the observations I gathered during this time:

* Even though I was able to unhide my user Library folder (using `chflags nohidden`), I wasn't able to search the Library folder from Finder using Spotlight. I verified that the folder wasn't excluded from Spotlight (there were no indications in the GUI that it was excluded). I was able to search the folder using `mdfind.`

* I found that even though Finder was configured to search the current window when performing a search, it always defaulted to searching my entire home directory. No amount of fiddling changed this behavior.

* Finder windows didn't "remember" their toolbar/sidebar state. Just about every time I'd open a Finder window, the toolbar and sidebar were visible, even though I'd turned off the toolbar and sidebar the last time I had the folder open.

* AppleScripts that I'd written to manipulate mail messages ran significantly slower against the version of Mail in Lion compared to Mail in Snow Leopard.

* The new version of Growl from the Mac App Store would consistently crash under Lion. These crashes were typically caused by Twitterrific. A matching configuration on my older laptop never demonstrated the same behaviors.

* Little things here and there throughout the interface felt sluggish. For example, there was always a noticeable delay between the sending of an e-mail message and the "whoosh" sound. That delay did not exist with Snow Leopard. This implied, IMHO, some sort of performance issue that prevented snappier sound performance.

* I personally found the minor UI changes (smaller window controls, for example, and changes in window shading between active and inactive windows) made the OS harder to use. Distinguishing between active and inactive windows was particularly problematic.

* Active windows would, from time to time, open behind an inactive window. There did not seem to be any noticeable pattern as to when this happened.

Because of all these various things, I found that the "frictionless" experience that I'd enjoyed in Mac OS X for years no longer existed. Lion, for me, now felt like Windows---the operating system was "getting in the way" and making it harder for me to get my work accomplished. To be fair, there were a few features that I really _did_ like (the new Spaces, some of the gestures, Resume), but those features didn't and couldn't outweigh the disadvantages. I was so disappointed.

By the time I arrived back in the States, I'd resolved that I would rebuild the laptop with Snow Leopard. Because this particular model of 13" MacBook Pro originally shipped with Snow Leopard, I knew that it would run properly (not the case with the new MacBook Air laptops, for example---they won't run Snow Leopard at all because of drivers that don't exist in 10.6.x). This past Sunday night, I rebuilt the 13" MacBook Pro with Snow Leopard. Since that time, the laptop and my applications have been rock solid and functional. It's surprising to me just how much of a difference I saw in usability and reliability between the original factory Lion install and my own clean Snow Leopard install. Snow Leopard has been, for me, worlds better.

So what's the key takeaway here? Apple wants to make Mac OS X "easier" and "simpler," to grow their installed base via the popularity of iOS, the iPhone, and the iPad. To that end, they shifted Lion closer to iOS and away from the UNIX roots that originally drew me to Mac OS X. Snow Leopard, on the other hand, retains the power and flexibility that were the key drivers for me to switch so many years ago (back in the days of the PowerPC G4 and Mac OS X 10.2k "Jaguar"). I suspect---although I don't have any hard facts---that many other technical professionals switched to the Mac for similar reasons. Will those users also be alienated by the changes in Lion? Is Apple pushing away the highly technical audience that has been one of their biggest proponents in the Mac market? For me, this shift toward iOS made Lion less appealing, less powerful, less useful for me. It introduced friction in my computing experience where previously there had been none.

Am I alone in my experience? Do you love Lion, or still prefer Snow Leopard? If you are a Mac user, I'd be interested in hearing your own experiences or thoughts regarding Lion and Snow Leopard. Feel free to speak up in the comments.

