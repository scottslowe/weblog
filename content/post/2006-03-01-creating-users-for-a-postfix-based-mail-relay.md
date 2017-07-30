---
author: slowe
categories: Information
comments: false
date: 2006-03-01T12:09:45Z
slug: creating-users-for-a-postfix-based-mail-relay
tags:
- Linux
- Messaging
- Postfix
- Security
title: Creating Users for a Postfix-Based Mail Relay
url: /2006/03/01/creating-users-for-a-postfix-based-mail-relay/
wordpress_id: 192
---

This is a really, really, _really_ simple task, but to save me the time of looking it up on those rare occasions when I need to do it I'm capturing the information here. This is how to create, delete, or modify users for a [Postfix](http://www.postfix.org/)-based mail relay using SASL.

All of these examples assume that SASL is configured to use "sasldb" as the authentication mechanism.

To create a new user, use the following syntax:

    saslpasswd2 -c -u <domain> <username>

For simplicity's sake, it's easiest to make both the domain and the username in the command above the same as the domain and the username in the user's e-mail address. This will make their full username the same as their e-mail address.

To change an existing user's password:

    saslpasswd2 -u <domain> <username>

This will prompt for password and password verification. To delete an existing user:

    saslpasswd2 -d -u <domain> <username>

Finally, to list the available users on the system, simply use:

    sasldblistusers2

This will list all the SASL users defined in the SASL database. Please note that the users' passwords will show up only as "userPassword", so it's not possible to see their existing passwords (at least, not without some effort).

There---now, the next time I need to do this, I'll be able to easily remember the instructions.
