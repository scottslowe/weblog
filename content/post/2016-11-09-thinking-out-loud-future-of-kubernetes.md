---
author: slowe
categories: Musing
comments: true
date: 2016-11-09T00:00:00Z
tags:
- Docker
- Linux
- Kubernetes
- ToL
title: 'Thinking Out Loud: The Future of Kubernetes'
url: /2016/11/09/thinking-out-loud-future-of-kubernetes/
---

I've just wrapped up KubeCon/CloudNativeCon 2016 in Seattle, WA. There's no doubt the [Kubernetes][link-1] community is active and engaged, and [the project itself][link-2] is charging forward. As both the community and the project grow, though, what does that mean for the future of Kubernetes?

Here are my thoughts, hopefully presented in a somewhat logical fashion.

It seems to me that Kubernetes has been successful thus far because of a strong focus on the problem it's trying to solve. You can see this in the Kubernetes web site, where phrases like "Production-Grade Container Orchestration" and "Automated container deployment, scaling, and management" are found. You can see this in the API abstractions Kubernetes uses (a _pod_ as a group of co-located containers, a _service_ as a stable access point for sets of pods, etc.). You can see it in the real-world customer deployments and use cases. Kubernetes seems focused on addressing the needs of container-based microservices-centric application architectures.

However, there now seem to be some efforts to push Kubernetes to support other types of applications as well. One could look at [DaemonSets][link-3] (which are used to ensure that a particular pod is always running on every node; useful for "infrastructure" services like logging, storage, monitoring, etc.) or [StatefulSets][link-4] (formerly PetSets; used for clustered applications that maintain state and need a stronger sense of identity) as examples. Are there use cases where these constructs are useful? Yes, certainly. Should these use cases attempt to be addressed by Kubernetes? I'm not so sure. The community thinks so, obviously, or we wouldn't have these features in the project.

All this brings us to these core questions:

* Does the inclusion of DaemonSets and StatefulSets represent an expansion of Kubernetes' scope? Or are these constructs simply a refining of what Kubernetes was always intended to do?
* Does an expansion of scope for Kubernetes mean a loss of focus on what has made it successful so far?
* If there is a loss of focus, what does that mean for the project?
* Will pushing Kubernetes to support other types of applications make it overly complex? Will that added complexity stop it from being effective in the area(s) that have made it successful so far?

One could make some [OpenStack][link-5] comparisons here (I have a separate post brewing on the future of OpenStack), but I'm not sure such a comparison would be valid. Unlike OpenStack, Kubernetes seems to have really strong technical guidance from a single source (Google), which may outweigh the pull of the community in multiple directions. It's entirely possible that the influence of a single strong source (Google) may help keep the project on-track and growing effectively. Time will tell.



[link-1]: http://kubernetes.io/
[link-2]: https://github.com/kubernetes/kubernetes/
[link-3]: http://kubernetes.io/docs/admin/daemons/
[link-4]: http://kubernetes.io/docs/user-guide/petset/
[link-5]: https://www.openstack.org/
