---
author: slowe
categories: Explanation
comments: true
date: 2020-04-04T19:45:00-07:00
tags:
- Firefox
- Linux
- Ubuntu
- Security
title: Making File URLs Work Again in Firefox
url: /2020/04/04/making-file-urls-work-again-in-firefox/
---

At some point in the last year or so---I don't know exactly when it happened---[Firefox][link-1], along with most of the other major browsers, stopped working with `file://` URLs. This is a shame, because I like [using Markdown for presentations][xref-1] (at least, when it's a presentation where I don't need to collaborate with others). However, using this sort of approach generally requires support for `file://` URLs (or requires running a local web server). In this post, I'll show you how to make `file://` URLs work again in Firefox.<!--more-->

I tested this procedure using Firefox 74 on [Ubuntu][link-2], but it should work on any platform on which Firefox is supported. Note that the locations of the `user.js` file will vary from OS to OS; see [this MozillaZine Knowledge Base entry][link-3] for more details.

Here's the process I followed:

1. Create the `user.js` file (it doesn't exist by default) in the correct location for your Firefox profile. (Refer to the MozillaZine KB article linked above for exactly where that is on your OS.)

2. In the `user.js`, add these entries:

        // Allow file:// links
        user_pref("capability.policy.policynames", "localfilelinks");
        user_pref("capability.policy.localfilelinks.sites", "file://");
        user_pref("capability.policy.localfilelinks.checkloaduri.enabled", "allAccess");

3. In your Firefox configuration (accessible using `about:config` in a Firefox tab), change the value of `privacy.file_unique_origin` from `true` to `false`.

4. Restart Firefox.

After you restart Firefox, you should be able to use `file://` URLs, but only from local HTML files on your system (as specified by the second line you added in step 2). It's possible this may expose an unknown security flaw or weakness that I haven't foreseen, so keep that in mind.

If you're a fan of Markdown-based presentations displayed using your browser, they should work again.

Hit [me on Twitter][link-5] if you have questions. Thanks!

[link-1]: https://www.mozilla.org/en-US/firefox/
[link-2]: https://ubuntu.com/
[link-3]: http://kb.mozillazine.org/Profile_folder_-_Firefox
[link-4]: https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS/Errors/CORSRequestNotHttp
[link-5]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2017-03-07-linux-migration-creating-presentations.md" >}}
