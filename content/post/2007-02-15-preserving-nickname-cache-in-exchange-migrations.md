---
author: slowe
categories: Education
comments: true
date: 2007-02-15T23:57:58Z
slug: preserving-nickname-cache-in-exchange-migrations
tags:
- Exchange
- Microsoft
- Outlook
title: Preserving Nickname Cache in Exchange Migrations
url: /2007/02/15/preserving-nickname-cache-in-exchange-migrations/
wordpress_id: 413
---

Migrating from one [Microsoft Exchange](http://www.microsoft.com/exchange/default.mspx) organization to another Exchange organization is always a troublesome task. There's free/busy time, public folders, the Global Address List (GAL), permissions, mailbox data, etc., to be worried out during the migration. With recent versions of the Outlook client, there's also the nickname cache to worry about, too.

The nickname cache is that functionality in Outlook that lets you start typing a recipient's name (one that you've used before) and Outlook will "auto-complete" the name for you. It's a pretty handy feature, to be honest, and I'm sure that quite a few users out there rely on this functionality. In fact, I've had people tell me that their nickname cache is more important than their contacts folder!

In a straight STMP-only situation, the nickname cache can be quite harmless during the migration. In an Exchange migration, however (such as when migrating from one organization to a new organization, perhaps due to acquisition, rebranding, whatever), the nickname cache can be quite a pain in the rear. Why? Because the nickname cache doesn't store the SMTP address of the Exchange recipients to which you've been sending e-mail; instead, it stores the X.500 address.

For example, when you mailbox-enable (i.e., create a mailbox for) a user object, Active Directory and Exchange will stamp the legacyExchangeDN attribute with an address that looks something like this:

    /o=VMware Test Lab/ou=Raleigh/cn=Recipients/cn=scott.lowe

This is an X.500 address, and if an Outlook user sends an e-mail to this mailbox-enabled user object, this X.500 address will get added to the nickname cache. For an Outlook user creates a contact for an Exchange recipient from the GAL, this X.500 address will be the address that is saved with the contact. If this mailbox is moved to a new organization, this X.500 address---by default---_won't go with it_, and that is the root cause of the most of nickname cache problems in a migration.

So how do we fix it? Easy, actually. Let's say that you want to represent John Doe, who has a mailbox in Organization B, in the GAL for Organization A. You create distinct SMTP namespaces for the two organizations, create SMTP connectors with the appropriate namespaces, and you test mail flow between the two. Everything works fine, and so you create contacts in the GAL for Organization A to be able to send e-mail messages to those recipients in Organization B.

Thinking you're pretty clever (which you likely are, since you're visiting this site), you create contacts in Organization A to represent users in Organization B and vice versa. Since these contacts are all SMTP based, routing messages based on the SMTP namespace, all should be well, right? Nope.

Unfortunately, because these contacts were created on the server (we're talking Active Directory Contact objects here, not Outlook contacts), users sending e-mail messages to them will be adding the X.500 address of the contact to their nickname cache, not the SMTP address. When these users migrate from Org A to Org B and send mail to these recipients again, the system will generate an NDR (non-delivery report). Why? Because the nickname cache has the X.500 address for the Contact object in the source AD tree.

But enough of the background. How do we fix the problem? Check out these steps:

1. Review the legacyExchangeDN attribute on the target mailbox.

2. Add the value of legacyExchangeDN (on the target mailbox) to an X.500 address on the Contact object in the source domain. To create an X.500 address, select the "Custom Address" and specify the address type as "X500" (no quotes).

That's it! When users send e-mail to the Contact objects, the X.500 address will be stored in the nickname cache. The Contact object's targetAddress attribute will, of course, point to an SMTP address assigned to the target mailbox; that's what allows Exchange Server to route the e-mail messages appropriately. After the users are migrated to the new Exchange organization, the X.500 address for the users to which they used to send mail will still be the same as the Contact objects they used to use. Perfect!
