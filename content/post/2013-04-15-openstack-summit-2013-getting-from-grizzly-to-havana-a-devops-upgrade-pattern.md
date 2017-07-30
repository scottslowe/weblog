---
author: slowe
categories: Liveblog
comments: true
date: 2013-04-15T12:23:23Z
slug: openstack-summit-2013-getting-from-grizzly-to-havana-a-devops-upgrade-pattern
tags:
- OpenStack
title: 'OpenStack Summit 2013: Getting from Grizzly to Havana, a DevOps Upgrade Pattern'
url: /2013/04/15/openstack-summit-2013-getting-from-grizzly-to-havana-a-devops-upgrade-pattern/
wordpress_id: 3132
---

This session is led by Rob Hirschfeld of Dell and---as the session title implies---focuses on orchestrating OpenStack upgrades. Rob, in case you were not aware, has been extensively involved in OpenStack and is a founder (co-founder?) of the Crowbar project.

Rob recommends reviewing Greg's talk on this same topic from the last Summit because, unfortunately, things haven't changed that much from then until now. He also recommends participating in the Reference Architecture (Tuesday @ 11:00 AM) and Interop (Tuesday @ 5:20 PM) sessions as well.

There is also a project, led by Piston, called Grenade, that is upgrade focused. Grenade, in Rob's view, doesn't adequately address multi-node upgrades, which is the focus of this session. The goal, of couse, is to enable upgrades while still enabling operators to keep their clouds up and running.

There are 3 basic ways to address the problem:

1. On the fly: This is the "continuous deployment" model

2. Split/Migrate/Replace: Upgrade nodes in staged groups/sets

3. Rolling Upgrade: Nodes upgraded individually by a system-wide orchestration system that supports multiple versions

DevOps is crucial here in order to support these models, especially the continuous deployment and rolling upgrade models. As OpenStack grows and matures, it grows in complexity and interdependencies (Rob gives an example of the dependencies if one wanted to upgrade Nova), and this increases the need for automation and orchestration.

According to Rob, it's crucial for operators (or teams like Rob's at Dell that focus on deployments) to be able to communicate in "near real-time" with developers so that issues that affect deployments and upgrades can be addressed as part of the development cycle, not at the end of the development cycle. Addressing issues quickly means it's easier to catch the individual commits and easier to fix potential issues. Working only at the major release level (like Grizzly to Havana) means there are more combinations to test and more changes at one time that need to be tested.

Rob uses a graphic (titled the "Migration Cube") which charts where the continuous deployment model fits along the axes of small steps vs. large steps, level of risk, and server vs. client upgrades. His point is that continuous deployment offers the least risk and the least disruption. By so doing, it also provides the most profit.

He applies the "pets vs. cattle" analogy to your servers, which is a different take (normally it's applied to VMs/instances). In that regard, it's a lot like the idea of "snowflake servers" vs. "phoenix servers" (I've mentioned this a few times in various presentations)--you don't want your servers to be carefully nurtured, lovingly crafted masterpieces. You want them to be easily repeatable, commodity resources. You want to treat them like cattle.

So how do we get the community to the continuous deployment model?

1. Servers and agents must be version tolerant.

2. Client protocols must be testable and documented.

3. Ensure non-destructive migrations.

4. Fast-fail on client, but version tolerant on server.

Upgrades need to stop being the "caboose"--it has to be more important and done up front.

At this point, Rob wraps up his session and opens it up for questions and answers.
