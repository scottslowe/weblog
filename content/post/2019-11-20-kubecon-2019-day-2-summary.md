---
author: slowe
categories: Information
comments: true
date: 2019-11-20T21:30:00-07:00
tags:
- Kubernetes
- KubeCon2019
title: KubeCon 2019 Day 2 Summary
url: /2019/11/20/kubecon-2019-day-2-summary/
---

## Keynotes

This morning's keynotes were, in my opinion, better than yesterday's morning keynotes. (I missed the closing keynotes yesterday due to customer meetings and calls.) Only a couple of keynotes really stuck out. Vicki Cheung provided some useful suggestions for tools that are helping to "close the gap" on user experience, and there was an interesting (but a bit overly long) session with a live demo on running a 5G mobile core on Kubernetes.

## Running Large-Scale Stateful Workloads

Due to some power outages at the conference venue resulting from rain in San Diego, the [Prometheus][link-3] session I had planned to attend got moved to a different time. As a result, I sat in this session by Lyft instead. The topic was about running large-scale stateful workloads, but the content was really about a custom solution Lyft built (called [Flyte][link-2]) that leveraged CRDs and custom controllers to help manage stateful workloads. While it's awesome that companies like Lyft can extend Kubernetes to address their specific needs, this session isn't helpful to more "ordinary" companies that are trying to figure out how to run their stateful workloads on Kubernetes. I'd really like the CNCF and the conference committee to try to promote talks that are more applicable to the general audience instead of these "Look what we built!" sessions.

## Leveling Up your CD: Unlocking Progressive Delivery on Kubernetes

This was a useful session covering [Argo][link-1] and some functionality being added to Argo to support what's being called _progressive delivery_. Progressive delivery is defined by them as continuous delivery with some fine-grained controls over the blast radius, and the presenters demonstrated new features in Argo that provide fine-grained control over rollouts and the use of canary deployments (they showed a feature whereby Prometheus metrics can be used to determine the validity of a canary deployment and roll it back if unsuccessful). Argo was already on my "list of things to dig into," but now I think it's moved up a few spots on that list.

I had on my schedule to attend a talk on [Krane][link-4], but the room moved from what was on my schedule (I don't know if this is related to the power outages in other areas of the conference venue or what). After realizing this and wandering around trying to find the room, I realized I'd missed too much of the session so gave up.

That's it for day 2. Tune in tomorrow for a recap on day 3, the final day of the event.

[link-1]: https://argoproj.github.io/
[link-2]: https://github.com/lyft/flyte
[link-3]: https://prometheus.io/
[link-4]: https://github.com/Shopify/krane
