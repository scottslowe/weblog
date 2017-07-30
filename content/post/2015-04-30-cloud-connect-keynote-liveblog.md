---
author: slowe
categories: Liveblog
comments: true
date: 2015-04-30T11:22:00Z
tags:
- Interop2015
- Cloud
title: 'Interop Liveblog: Thursday Cloud Connect Keynote'
url: /2015/04/30/cloud-connect-keynote-liveblog/
---

This is a liveblog of the Thursday morning Cloud Connect keynote at Interop 2015 in Las Vegas. The title of the presentation is "Doing it Live," and the speaker is Jared Wray ([@jaredwray][link-1] on Twitter; he's Cloud CTO and SVP of Platform at CenturyLink).

As the session kicks off, Wray shares that his presentation was drastically altered, a nod to the drastic changes that he is seeing at CenturyLink. He then shares a bit of background on him, his history in IT, and the events that brought him to CenturyLink. Wray then spends a few minutes talking about CenturyLink and CenturyLink's services, which he insists "isn't a product pitch" (it feels like one). The key tenets of CenturyLink's offerings are that they are fully automated; they are programmable; and they are self service.

Wray points out that CenturyLink's transformation to next generation platform services and containers requires that they also transform their operations (and people, though that is called out separately).

According to Wray, the blanket "move everything to the cloud" doesn't work; enterprises must embrace a "cap and grow" strategy. This means not moving applications if there is no benefit (and also moving applications to maintenance mode until there is a benefit). There's also a lot of investigation required to move applications to the cloud or to transform them into cloud services.

Part of this also means defining IaaS, PaaS, and SaaS for individuals within older companies (CenturyLink is 85 years old, if I heard Wray correctly). You can't assume that the company understands the terms, technologies, and techniques you're describing (or even the benefits that might come from these technologies and techniques).

Wray also shares that a lot of work had to go into actually deciding how things would be architected and built moving forward. This includes requiring architectures that leverage APIs ("API first"), heavy use of virtualization and containers (lots of CloudFoundry and Docker use internally), standardization on the use of continuous development (includes use of Chef, Ansible, Docker, and several internally-developed tools), the full use of automation ("Robots vs. Oompa Loompas"), and lots of changes to culture.

The culture changes ("Robots vs. Oompa-Loompas", for example) were a lot of work, according to Wray. He used Gene Kim's _The Phoenix Project_ as a way of helping to force a different way of thinking within the organization, and found strong support in CenturyLink's CIO (which really helped). Once culture started changing, then it was time to change the process. This included dramatic changes to how products were developed and owned, and removing lots of layers of management (aka "overhead" according to Wray). Eliminating titles is actually quite effective, Wray shares.

And with that, Wray wraps up his session.


[link-1]: https://twitter.com/jaredwray
