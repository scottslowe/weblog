---
author: slowe
categories: Explanation
comments: false
date: 2006-07-28T23:13:16Z
slug: active-directory-and-vas
tags:
- ActiveDirectory
- Interoperability
- LDAP
- Microsoft
- UNIX
title: Active Directory and VAS
url: /2006/07/28/active-directory-and-vas/
wordpress_id: 308
---

The problem first presented itself as latency in responses from the domain controllers (DCs) to the Exchange servers and Outlook clients, resulting in slow responses in the Outlook client, delays in receiving new e-mail messages, etc. Upon closer inspection, we determined that the problem was excessive CPU utilization on the DCs. As we examined the DCs more closely, we then determined that traffic from the customer's UNIX servers were driving up the CPU usage on the DCs.

After a great deal of troubleshooting and a lengthy call to Quest Technical Support, it was determined that the key problem was the length of the LDAP queries that were being performed---some taking up to 3 or 4 seconds _for a single query_. The underlying cause of this delay was thought to be the Active Directory attribute selected as the user login attribute. In this case, the customer had selected UID (an attribute added by the VAS schema extension) as the user login attribute. Unfortunately, UID is an attribute that is not indexed in Active Directory, and it is believed that this is the key problem in the delays of the queries and the excessive CPU utilization caused by the queries. The fix is simply to index this attribute, a change that can be made relatively easily via the Schema Management snap-in.

&lt;aside&gt;The default user login attribute for VAS is the user principal name (UPN), which is an indexed attribute. If the VAS installation is going to use a non-indexed attribute as the user login attribute, however, you could see the same kinds of excessive delays and CPU usage.&lt;/aside&gt;

Before the customer puts this change (indexing the UID attribute) into production, we're going to test the change in a development environment and try to see if this addresses some of the symptoms. Once the results of the testing are available, I'll post an update here.

**UPDATE:** Testing in lab today with the customer showed a definite, marked, and quite noticeable improvement in query performance after indexing the UID attribute in Active Directory. This improvement in performance was repeatable as well; when we removed the index for uID, the delays and increased CPU utilization returned immediately. So, make sure the user logon attribute in your VAS deployments is indexed in Active Directory.
