---
author: slowe
categories: Information
comments: true
date: 2012-11-30T16:18:33Z
slug: problem-with-markdown-files-in-boxcryptor-on-ios
tags:
- Encryption
- Interoperability
- macOS
- iOS
- OSS
- Privacy
- Security
- Markdown
title: Problem with Markdown Files in BoxCryptor on iOS
url: /2012/11/30/problem-with-markdown-files-in-boxcryptor-on-ios/
wordpress_id: 2982
---

About a week ago, I published an article showing you how to [use EncFS and BoxCryptor to provide client-side encryption of Dropbox data][1]. After working with this configuration for a while, I've run across a problem (at least, a problem for me---it might not be a problem for you). The problem lies on the iPad end of things.

If you haven't read the earlier post, the basic gist of the idea is to use EncFS---an open source encrypting file system---and OSXFUSE to provide file-level encryption of Dropbox data on your OS X system. This is client-side encryption where you are in the control of the encryption keys. To access these encrypted files from your iPad, you'll use the [BoxCryptor iOS client](https://www.boxcryptor.com/), which is compatible with EncFS and decrypts the files.

Sounds great, right? Well, it ismostly. The problem arises from the way that the iPad handles files. BoxCryptor uses the built-in document preview functionality of iOS, which in turn allows you to access the iPad's "Open In" functionality. The **only** way to get to the "Open In" menu is to first preview the document using the iOS document preview feature. Unfortunately, the iOS document preview functionality doesn't recognize a number of files and file types. Most notably for me, it doesn't recognize Markdown files (I've tried several different file extensions and none of them seem to work). Since the preview feature doesn't recognize Markdown, then I can't get to "Open In" to open the documents in Byword (an iOS Markdown editor), and so I'm essentially unable to access my content.

To see if this was an iOS-wide problem or a problem limited to BoxCryptor, I tested accessing some non-encrypted files using the Dropbox iOS client. The Dropbox client will, at least, render Markdown and OPML files as plain text. The Dropbox iOS client still does not, unfortunately, know how to get the Markdown files into Byword. I even tried a MindManager mind map; the Dropbox client couldn't preview it (not surprisingly), but it did give me the option to open it in the iOS version of MindManager. The BoxCryptor client also worked with a mind map, but refuses to work with plain text-based files like Markdown and OPML.

Given that I create the vast majority of my content in Markdown, this is a problem. If anyone has any suggestions, I'd love to hear them in the comments. Otherwise, I'll post more here as soon as I learn more or find a workaround.

[1]: {{< relref "2012-11-23-using-encfs-with-dropbox-and-boxcryptor.md" >}}
