---
author: slowe
categories: Information
comments: true
date: 2019-11-19T18:00:00-07:00
tags:
- Kubernetes
- KubeCon2019
title: KubeCon 2019 Day 1 Summary
url: /2019/11/19/kubecon-2019-day-1-summary/
---

This week I'm in San Diego for KubeCon + CloudNativeCon. Instead of liveblogging each session individually, I thought I might instead attempt a "daily summary" post that captures highlights from all the sessions each day. Here's my recap of day 1 at KubeCon + CloudNativeCon.<!--more-->

## Keynotes

KubeCon + CloudNativeCon doesn't have "one" keynote; it uses a series of shorter keynotes by various speakers. This has advantages and disadvantages; one key advantage is that there is more variety, and the attendees are more likely to stay engaged. I particularly enjoyed Bryan Liles' CNCF project updates; I like Bryan's sense of humor, and getting updates on some of the CNCF projects is always useful. As for some of the other keynotes, those that were thinly-disguised vendor sales pitches were generally pretty poor.

## Introduction to the Virtual Kubelet

I was running late for the start of this session due to booth duty, and I guess the stuff I needed most was presented in that portion I missed. Most of what I saw was about Netflix Titus, and how the Netflix team ported Titus from Mesos to Virtual Kubelet. However, that information was so specific to Netflix's particular use of Virtual Kubelet that it wasn't all that helpful (for me, anyway).

## TGIK Live

Joe Beda did a "TGIK Live" session on the show floor, discussing [CUE][link-1]. CUE's been on my list to evaluate more closely (and possibly blog about). Joe's demonstration of CUE helped provide a bit of a foundation for CUE, but I still have a lot of learning to do before I'll be ready to write about it.

## Russian Doll: Extending Containers with Nested Processes

Due to customer meetings and such, this was the only afternoon session I was able to attend in its entirety (I did manage to catch part of the session on Vitess). This session was presented by a couple of Google employees, and focused around efforts to run containers within a Pod in a sequential fashion (in a more complex fashion than `initContainers` support). The use case was to support tasks in [Tekton][link-2], and the solution (or hack?) was to use a Go binary that "waits" on previous containers before running the original container `ENTRYPOINT`. I understand the use case, but I also have to wonder if a CustomResourceDefinition wouldn't have been more appropriate for this.

## Vitess: Stateless Storage in the Cloud

I was only able to capture the first portion of this session, which was about the [Vitess][link-3] project (a MySQL-compatible, scale-out, cloud-native database). The Vitess project, which is hosted by CNCF, announced today that the project has graduated (putting it on par with Kubernetes and Prometheus and others). The presenter shared the story---often with humorous anecdotes---of how Vitess came to be. I was hoping for some technical depth, but didn't get any before I had to step out due to a customer meeting.

That's it for day 1. Tune in tomorrow for a day 2 recap!

[link-1]: https://cuelang.org/
[link-2]: https://tekton.dev/
[link-3]: https://vitess.io/
