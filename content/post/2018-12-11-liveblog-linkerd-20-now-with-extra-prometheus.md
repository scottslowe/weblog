---
author: slowe
categories: Liveblog
comments: true
date: 2018-12-11T13:00:00Z
tags:
- KubeCon2018
- Prometheus
- Linkerd
title: "Liveblog: Linkerd 2.0, Now with Extra Prometheus"
url: /2018/12/11/liveblog-linkerd-20-now-with-extra-prometheus/
---

This is a liveblog of the KubeCon NA 2018 session titled "Linkerd 2.0, Now with Extra Prometheus." The speakers are Frederic Branczyk from Red Hat and Andrew Seigner with Buoyant.<!--more-->

Seigner kicks off the session with a quick introduction before handing off to Branczyk. Prometheus, for folks who didn't know, originated at SoundCloud with a couple of ex-Googlers. Prometheus is one of the graduated CNCF projects and---judging by a show of hands in response to a speaker question---lots of folks here at KubeCon know about Prometheus and are using Prometheus in production.

Branczyk provides an overview of Prometheus, explaining that it pulls metrics from a target on a set of regular intervals (like every 15 seconds, for example). Prometheus stores those metrics in a time-series database, so every time it pulls metrics it stores them in a time series. As a monitoring solution, it also has to provide alerting, to notify cluster operators/administrators that some metric is outside of some predefined threshold.

With regards to Kubernetes, Prometheus has built-in support to perform service discovery in Kubernetes by querying the Kubernetes API. This enables it to discover Pods backing a Service and scrape (pull) the metrics from those discovered Pods. Branczyk also spends some time talking about time series churn and a series of changes that went into Prometheus 2.0 to help address this and other challenges around gathering time-series data from ephemeral data sources (Pods).

A single Prometheus server has pretty amazing scale; Branczyk indicates he's seen a single server ingesting 700K samples per second (which translates, roughly, to a Kubernetes cluster with 350 nodes).

At this point, Branczyk hands it over to Seigner, who shifts the focus to Linkerd. Seigner provides an overview of Linkerd. Linkerd originated at Twitter, as part of their effort to decompose the Twitter application into microservices. As part of that decomposition process, the people at Twitter realized there was a common set of functionality that all services needed, and this led to the creation of Linkerd to provide common functionality via a service mesh and a sidecar proxy.

But Linkerd 1.x was written in Scala, but Scala was a bit heavyweight. As part of the re-write from Linkerd 1.x to Linkerd 2.0, the proxy was re-written in Golang (for the control plane) and Rust (for the data plane). Linkerd 2.0 also incorporated native Kubernetes support and integrated support for Prometheus. This integrated support means Prometheus can scrape metrics from every Linkerd proxy running as a sidecar, which gives access to lots of very useful metrics.

This leads into a quick demo of Linkerd to show its usage with a simple microservices-based application. The demo shows the use of the `linkerd` CLI tool to install Linkerd onto a Kubernetes cluster, and then to inject Linkerd into the existing microservices-based application. The `linkerd` command also includes a ton of sub-commands that can show staticstics, show summary statistics, and expose a graphical web-based dashboard.

At this point, Seigner wraps up the session and opens up for questions.
