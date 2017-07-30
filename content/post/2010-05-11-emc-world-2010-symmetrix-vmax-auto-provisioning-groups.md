---
author: slowe
categories: Liveblog
comments: true
date: 2010-05-11T09:33:03Z
slug: emc-world-2010-symmetrix-vmax-auto-provisioning-groups
tags:
- EMC
- EMCWorld2010
- Storage
- Symmetrix
title: 'EMC World 2010: Symmetrix VMAX Auto-Provisioning Groups'
url: /2010/05/11/emc-world-2010-symmetrix-vmax-auto-provisioning-groups/
wordpress_id: 1938
---

Here are a few notes from another Symmetrix session I attended Monday afternoon. It's not a complete session blog (the presenter was presenting so much information I couldn't capture it all).

Auto-provisioning groups include storage groups, port groups, and initiator groups. A storage group is just a collection of storage resources that are presented to a cluster of hosts. Similarly, a port group is a collection of front-end ports, and initiator groups are just groups of hosts.

These three groups are combined together into a masking view. The masking view ensures that the desired initiators (in an initiator group) have access to the desired storage resources (in a storage group) through the desired ports (in a port group).

The SYMCLI `symaccess` command perform all the necessary auto-provisioning functions. You can also use SMC to perform the tasks.

A storage group is simply a collection of Symmetrix logical volumes that are used by an application, a server, or a collection of servers. Storage groups are not only used for presenting storage to hosts, but also for Fully Automated Storage Tiering (FAST) policies.

A port group is just a collection of front-end director ports that are used together.

An initiator group is just a collection of host bus adapters (HBAs) that are used together. This could be all the HBAs on a server, all the HBAs in a group of servers, or some other arbitrary collection of initiators. Initiator groups can contain other initiator groups (called cascaded initiator groups).

Masking views aren't only for provisioning storage; they are also helpful in making changes to storage presentation. Changes to groups contained in a masking view are automatically reflected in the masking view. Adding a storage device to the storage group, adding a port to the port group, or adding an initiator to the initiator group automatically updates the masking view.

This greatly simplifies storage allocation and de-allocation.

Cascaded initiator groups (initiator groups that contain other initiator groups) can be useful in presenting storage to clustered resources. For example, you might have an initiator group for all the HBAs in a server. This initiator group can then be used to present a boot LUN. The same host-specific initiator group can then be combined with other host-specific initiator groups into a cascaded group, and the cascaded group can be put into a masking view to present storage resources to the entire cluster at once.

Groups (and masking views) can combine single objects (a single HBA, a single front-end director port, a single storage resource) or multiple objects. Either approach is valid.

There is a compatibility mode with auto-provisioning groups that allows continued use of the `symmask` command. Even with compatibility mode, there are still changes that will likely need to be made to existing scripts.
