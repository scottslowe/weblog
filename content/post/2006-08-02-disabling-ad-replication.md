---
author: slowe
categories: Tutorial
comments: true
date: 2006-08-02T13:11:15Z
slug: disabling-ad-replication
tags:
- ActiveDirectory
- Microsoft
title: Disabling AD Replication
url: /2006/08/02/disabling-ad-replication/
wordpress_id: 312
---

Replication is bidirectional, occurring both inbound and outbound. Each of these directions can be disabled/enabled indepedently of the other using the `repadmin` command. The `repadmin` command is part of the support tools, included on the Windows 2000 and Windows Server 2003 CDs but not installed by default. (Installing them is highly recommended in all situations.)

To disable outbound replication from a particular DC, use this command:

    repadmin /options <DC NAME> +DISABLE_OUTBOUND_REPL

Likewise, to disable inbound replication for a particular DC, use this command:

    repadmin /options <DC NAME> +DISABLE_INBOUND_REPL

In these commands, we are _adding_ the "DISABLE_OUTBOUND_REPL" or "DISABLE_INBOUND_REPL" flag to the DC, so that running `repadmin /options` will show that flag as an option on the selected DC. To re-enable replication, then, we need to _remove_ the flag using one of the two commands:

    repadmin /options <DC NAME> -DISABLE_OUTBOUND_REPL<br></br>
    repadmin /options <DC NAME> -DISABLE_INBOUND_REPL

When replication is disabled, warning events 1115 (for disabled outbound replication) or 1113 (for disabled inbound replication) from source NTDS General will be logged in the Directory Service event log during system startup. As far as I am aware, no events are regularly logged during normal operation to indicate that replication is disabled. When replication is re-enabled, informational events 1116 (for outbound replication) and 1114 (for inbound replication) are logged.

When replication is disabled, NTDS KCC warning events (typically with event ID 1265) will be logged; the text of the message will provide information on the specific DCs and naming contexts involved, but the useful information is near the end of the event, where the message states that "The destination/source server is currently rejecting replication requests." If you see this, make sure that replication is enabled by searching the Directory Service event log for messages indicating that replication has been disabled.
