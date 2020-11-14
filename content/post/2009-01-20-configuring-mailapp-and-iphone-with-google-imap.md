---
author: slowe
categories: Explanation
comments: true
date: 2009-01-20T12:16:30Z
slug: configuring-mailapp-and-iphone-with-google-imap
tags:
- iPhone
- macOS
- iOS
- Messaging
title: Configuring Mail.app and iPhone with Google IMAP
url: /2009/01/20/configuring-mailapp-and-iphone-with-google-imap/
wordpress_id: 1139
---

So I recently moved almost all of my personal e-mail domains over to Google Apps. A couple of people have asked, "Why?" My answer is simple: it's easier. The e-mail functionality of my current hosting provider is lacking in a couple of key areas:

* Rather than using the emerging standard of having e-mail clients connect to TCP port 587 (Submission) to send e-mail, they used a very non-standard practice of using TCP port 26. (Now if we could just get older versions of Outlook to not have a severely broken SMTP client implementation, we'd be in good shape. But that's another story...)

* Despite paying for a dedicated IP address, I can't use my own SSL certificates for e-mail (only web traffic). The SSL certificates the hosting provider supplies for e-mail are self-signed certificates and cause fits to clients such as Outlook and Mac's Mail.app.

By using Gmail and/or Google Apps, on the other hand, these issues go away. However, Google's particular implementation of IMAP---and its use of labels vs. folders---presents a few challenges of its own. During the process of migrating over to Google Apps and using IMAP for all my e-mail accounts, I have finally settled into a configuration that works well for managing e-mail from my MacBook Pro as well as my iPhone.

The secret lies in a Google Labs feature called "Advanced IMAP Controls." By enabling Advanced IMAP Controls, Google Apps and Gmail users can control which labels will appear in Mail.app (and other IMAP clients, like the iPhone). Here's the configuration I've been using that seems to work really well:

1. In the Mail section of Google Apps or Gmail, go to Settings, then Labs, and enable "Advanced IMAP Controls". Google Apps users may need their administrator (if they don't have administrative permissions) to allow Labs features to appear. I'm not sure about Gmail users; I think Labs features are available by default for Gmail users.

2. Once Advanced IMAP Controls are enabled, go to the "Labels" section of Settings and **uncheck** all labels except Drafts, Spam, and Trash.

3. When setting up Mail.app, configure the IMAP account as normal, but set the Inbox Path Prefix to "[Gmail]". When you take the account online, a heading for that account should appear in the Mail.app sidebar with three folders under it: Drafts, Trash, and Spam.

4. Select the Drafts folder/label under the account's heading, then go to Mailbox > Use This Mailbox For > Drafts. This should cause the Drafts folder under the account's heading to disappear. Instead, it will be listed under the unified Drafts folder under the Mailboxes heading.

5. Repeat the process for the Trash folder/label (use for Trash) and Spam folder/label (use for Junk). After performing this process on all three folders/labels, the account heading should disappear from Mail.app's sidebar.

6. In the Mailbox Behaviors section of the account settings (Under Mail > Preferences) check the box for "Store draft messages on the server."

7. In the same area, also check "Store junk messages on the server" and specify a time period for how long to keep junk messages.

8. Finally, check the box for "Move deleted messages to the Trash mailbox" and "Store deleted messages on the server" and specify how long to keep deleted messages.

To keep mail synchronized between the IMAP server, Mail.app on my laptop, and Mail on my iPhone, I replicated these settings on my iPhone, selecting the Drafts folder/label as the "Drafts Mailbox" and the Trash folder/label as the "Deleted Mailbox" in the Advanced area of Mail settings.

With this configuration, reading a message on my laptop will mark it as read on my iPhone, and deleting a message on my iPhone will make it appear in the Trash mailbox on my laptop. In addition, I can continue to leverage Gmail/Google App's web interface when necessary as well, and see draft messages and deleted messages in the appropriate areas there, too. All in all, it works very well for me.

If you have other tips for enhancing the use of Gmail/Google Apps with Mail.app and your iPhone, I'd love to hear them in the comments below. Thanks!
