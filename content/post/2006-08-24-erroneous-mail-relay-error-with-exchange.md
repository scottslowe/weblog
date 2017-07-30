---
author: slowe
categories: Explanation
comments: true
date: 2006-08-24T16:03:48Z
slug: erroneous-mail-relay-error-with-exchange
tags:
- ActiveDirectory
- Exchange
- Microsoft
title: Erroneous Mail Relay Error with Exchange
url: /2006/08/24/erroneous-mail-relay-error-with-exchange/
wordpress_id: 326
---

You've probably all run into the "Unable to relay" error message before. The usual fixes to this problem are pretty straightforward:

* Add the address space to a recipient policy in Exchange System Manager; this tell Exchange to accept the messages as inbound; or

* Add IP addresses, subnets, or host names to the SMTP virtual server in Exchange System Manager as allowed to relay through this host;

Generally speaking, either one of these two options will generally fix the problem. If the address space (say, example.com) should truly be accepted inbound, then adding it to an Exchange recipient policy will tell Exchange to accept it as inbound---thus eliminating the relay issue. Likewise, if the messages should truly be relayed to their final destination, then adding the host(s) to the list of machines allowed to relay via the Exchange SMTP virtual server will indeed do that. Pretty simple, right?

In this case, the e-mail address space should have been accepted inbound, so I first added the address space to an Exchange recipient policy (leaving the check box to apply that address space to recipients _unchecked_, since I didn't really want addresses from this domain applied to users). That didn't work. OK, maybe the check box to apply this domain needs to be checked, so I add it to a recipient policy that has no filter criteria (and therefore won't be applied to any users). It still doesn't work.

Next, I add the appropriate servers to the list of hosts allowed to relay (by modifying the properties of the Exchange SMTP virtual server), restart the SMTP service, and try again. It _still_ doesn't work, reporting a "5.7.1 Unable to relay for &lt;username&gt;" message.

My colleague searched the Microsoft Knowledge Base and turns up [KB article 323669](http://support.microsoft.com/?id=323669), titled "XFOR: 550 5.7.1 Unable to relay Error Message When E-Mail is Sent to Local Exchange Recipient". In the article, it mentions using [MetaEdit](http://support.microsoft.com/default.aspx?scid=KB;en-us;q232068) to edit the IIS metabase and delete the LM/DS2MB key.

Good idea, but MetaEdit doesn't support IIS 6.0, and this problem is occurring on [Windows Server 2003](http://www.microsoft.com/windowsserver2003/default.mspx) with [Exchange Server 2003](http://www.microsoft.com/exchange/default.mspx). Fortunately, the [IIS 6.0 Resource Kit](http://www.microsoft.com/downloads/info.aspx?na=22&p=2&SrcDisplayLang=en&SrcCategoryId=&SrcFamilyId=&u=%2fdownloads%2fdetails.aspx%3fFamilyID%3d80a1b6e6-829e-49b7-8c02-333d9c148e69%26DisplayLang%3den) comes with the Metabase Explorer, which allows me to delete the key as per the KB article and restart the services. After restarting the services, the problem is fixed! (Kudos go to Chauncey, who found both the article and the Metabase Explorer.)

Apparently, the DS2MB portion is involved in one-way replication of configuration data from Active Directory (the directory service, hence the "DS" in the name) to the metabase. If the metabase (or even just the DS2MB portion of the metabase) gets damaged, then changes made in Exchange System Manager---which are written to Active Directory via the configuration domain controller---will never get synchronized to the IIS metabase. Since the IIS metabase controls the SMTP service, then SMTP will continue to deny what it believes to be unallowed requests to relay. Removing the DS2MB key and restarting the services rebuilds that portion of the metabase from scratch, thus pulling in the correct configuration from Active Directory and fixing the problem.
