---
author: slowe
categories: Liveblog
comments: true
date: 2018-12-11T15:00:00Z
tags:
- KubeCon2018
- Kubernetes
- Security
title: "Liveblog: Hardening Kubernetes Setups"
url: /2018/12/11/liveblog-hardening-kubernetes-setups/
---

This is a liveblog of the KubeCon NA 2018 session titled "Hardening Kubernetes Setup: War Stories from the Trenches of Production." The speaker is Puja Abbassi ([@puja108][link-1] on Twitter) from Giant Swarm. It's a pretty popular session, held in one of the larger ballrooms up on level 6 of the convention center, and nearly every seat was full.<!--more-->

Abbassi starts by talking about Giant Swarm's environment, in which they run more than 100 clusters across different clouds and different regions. These clusters are running for different companies, different industries, and they serve different use cases for various constituents of users. Abbassi says that Giant Swarm opts to give users more freedom in how they use (and potentially misuse) the clusters.

Obviously, this can lead to problems, and that's where the postmortems come into play. Abbassi explains the idea behind postmortems by quoting a definition from the Google SRE book, and then provides some context about the process that Giant Swarm follows when conducting postmortems. That leads into a discussion of various postmortems conducted at Giant Swarm.

The first one mentioned by Abbassi concerns a memory leak first fixed in 1.11.4 and 1.12.0. Prior to the fix, the memory leak could cause downtime due to failures of the control plane components.

(Side note: I was thinking that this session was security-related, but apparently it is not.)

The next one mentioned was an issue with Calico that was causing IP-in-IP tunnels to go down. Giant Swarm fixed this by writing a small ICMP pinger that generated enough traffic to keep the tunnels up.

After sharing a couple examples from Giant Swarm postmortems, Abbassi moves into a review of some hardening steps and best practices. Some of the hotspots for issues included flaws in older (non-upgraded) Kubernetes clusters, ingress, networking and DNS, resource (CPU or memory) pressure, and multi-tenancy. Next, Abbassi delves into a bit more detail on these hotspots:

* Old issues typically have issues that have been solved in newer versions. Customers should test upgrades extensively, and automate the upgrade process in order to reduce the "cost" of upgrading so that your organization is more likely to upgrade on a regular basis.
* Ingress: When it comes to the Nginx ingress controller, newer versions are less prone to misconfiguration. Users may also want to consider running multiple classes of ingress controllers, to help separate traffic types and protect against failures. Performing load testing and failover testing is also strongly recommended.
* Networking and DNS: Be sure to monitor and alert on network health and connectivity. Monitor DNS latency (via Prometheus?). Check for known issues, and apply best practices. (Abbassi stops short of actually discussing some best practices.)
* Resource pressure: Be sure to use resource management (use quotes, limits, and requests). Be aware many Java-based apps in containers don't behave well. Include buffers (extra headroom) in resource capacity. Be sure to protect K8s and critical addons, so that the Kubernetes infrastructure isn't crippled.
* Multi-tenancy: Namespaces and _proper_ RBAC configuration can help here. Avoid the use of the cluster-admin role! Use separate clusters where possible, and automate application deployment with CI/CD. Minimize manual operations as much as you can.

Finally, Abbassi provides a few "best practices":

* Use a monitoring/alerting solution, like Prometheus with Alert Manager.
* Logging---and sometimes tracing---can help with debugging.
* Be sure to fix issues fast; don't let them escalate and cause even bigger/more significant issues.
* Educate users on the correct/preferred way to interact with and/or use Kubernetes.
* Have a postmortem process (and learn from it!).
* Train on recovery, so that staff are able to more quickly get back to operational. (Heptio Ark gets a mention here as a solution.)

Finally, Abbassi mentions that many of the attendees are "standing on the shoulders of giants"---be sure to leverage the learnings that many others have already encountered and shared with the broader community. There's no need to reinvent the wheel, and don't be afraid to ask for help!

Abbassi closes out the session with thanks to his team at Giant Swarm, and then opens up for questions.

[link-1]: https://twitter.com/puja108
