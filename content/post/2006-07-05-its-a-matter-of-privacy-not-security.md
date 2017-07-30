---
author: slowe
categories: Rant
comments: false
date: 2006-07-05T14:18:59Z
slug: its-a-matter-of-privacy-not-security
tags:
- Apple
- Macintosh
- Privacy
- Security
title: It's a Matter of Privacy, Not Security
url: /2006/07/05/its-a-matter-of-privacy-not-security/
wordpress_id: 291
---

Daniel Jalkut, in his [Red Sweater Blog](http://www.red-sweater.com/blog/), recently posted that he had detected (via Little Snitch) some [network activity from Dashboard](http://www.red-sweater.com/blog/153/apple-phones-home-too) back to Apple's web site. Upon further investigation, he found that the activity was apparently tied to this one-line entry in the release notes for 10.4.7:

>You can now verify whether or not a Dashboard widget you downloaded is the same version as a widget featured on (www.apple.com) before installing it.

To me, that doesn't do an adequate job of informing the end user that the computer will be contacting Apple on a regular basis to verify the installed widgets. What it says is that the OS _can_ (i.e., has the ability to), upon the user's request, verify a widget before installing it. Those are two different things.

A lot of the comments on Daniel's entry are blasting him for posting this information, stating that it's a matter of security, that Apple is providing functionality to help protect Mac users against malicious code. OK, I'll grant that the ability to verify the authenticity of a downloaded widget is a good idea. I'll even grant that the ability to manually, whenever I feel like it, ask my computer to verify the authenticity of the currently installed widgets is a good idea. I'll even go so far as to grant that having a checkbox somewhere that says, "Periodically check my installed widgets for authenticity" or something similar would be a good idea.

What's _not_ a good idea is adding a "phone home" feature that users (apparently) can't disable and that can't be configured, controlled, or adjusted. What's further _not_ a good idea is misrepresenting this functionality in the release notes. Finally, what's _not_ a good idea is for Mac users to confuse security with privacy.

This isn't a matter of security. Most everyone agrees that having the ability to check widgets to make sure they are safe is a good idea. The problem here is that our computers are now communicating with Apple in a way that we did not authorize, were not informed about, and can not control. That makes it a matter of privacy, not security. And yes, while the communication right now is benign, will it always be so? What will the Mac users do when it is not benign?

I posted a comment to the article to see what methods, if any, are available to disable this functionality. As soon as I get some additional information, I'll post it here.

**UPDATE:** This safety check can be disabled by using the command:

    sudo launchctl unload -w /System/Library/LaunchDaemons/ 
    com.apple.dashboard.advisory.fetch.plist

(This should all be typed on a single line.) In addition, it's important to note that the dashboardadvisoryd process does not _send_ any information to Apple currently; it only fetches information from Apple and compares it to the list of currently installed widgets. Also, in the light of the extensive information shared with Apple as a result of using Software Update (an aspect I did not consider originally), I retract most of my concerns regarding privacy. I do, however, stand by the statement that Apple should have been more informative and forthcoming with information on exactly how this work, as well as given users a means whereby to control it.
