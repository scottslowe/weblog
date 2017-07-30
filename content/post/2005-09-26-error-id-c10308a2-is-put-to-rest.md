---
author: slowe
categories: Information
comments: false
date: 2005-09-26T13:45:18Z
slug: error-id-c10308a2-is-put-to-rest
tags:
- ActiveDirectory
- Exchange
- Microsoft
- Windows
title: Error ID c10308a2 is Put to Rest
url: /2005/09/26/error-id-c10308a2-is-put-to-rest/
wordpress_id: 94
---

As you may already know, I've been struggling with a bug in an environment running Exchange Server 2003 and Windows Server 2003 with SP1. The bug is manifested as an error that appears when users with the properly delegated permissions attempt to add or modify an e-mail address on an already mail-enabled or mailbox-enabled object. The error, listed as error ID c10308a2, contains text along the lines of being unable to determine if the Microsoft Exchange System Attendant service is running.

The underlying issue is a change that Windows Server 2003 SP1 makes to the security descriptors applied to the Service Control Manager. This change in security descriptors now makes it impossible for non-administrators to query service status; hence, the error message.

In trying to apply the fix suggested by Microsoft in KB905809 (the use of the `SC.EXE` command), the error was never resolved. It turns out that the workstation I was using the test environment was configured not to use the primary DNS suffix, but instead use a predetermined DNS suffix search list. This configuration resulted in the system's AD domain name **_not_** being in the suffix search list. As a result, even though the fix from Microsoft was applied, errors still occurred.

This morning I double-checked everything on the test servers as well as the test workstations, corrected the problem described above, and reset the environment to match the production environment. Then, walking through the tests again, I confirmed that running the `SC.EXE` command to add permissions to the Authenticated Users group (see the KB article linked above for more details, then see [this explanation](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/secauthz/security/security_descriptor_string_format.asp) of SDDL syntax) **_does_** indeed resolve the issue.

So, finally, we can put this issue to rest. If you are running Exchange Server 2003 with Windows Server 2003 SP1 and finding that your non-administrative users can't add or modify e-mail addresses using Active Directory Users & Computers, see KB905809 and run the `SC.EXE` command listed in there.
