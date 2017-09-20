---
author: slowe
categories: Liveblog
comments: true
date: 2017-09-20T15:00:00Z
tags:
- HashiConf2017
- Kubernetes
title: HashiConf 2017 Day 2 Keynote
url: /2017/09/20/hashiconf-2017-day-2-keynote/
---

This is a liveblog of the day 2 keynote (general session) at HashiConf 2017 in Austin, TX. Speakers today will (apparently, based on the schedule) include someone from Amazon Web Services and Kelsey Hightower from Google.<!--more-->

The keynote starts off with a photo montage of attendees, sessions, and speakers from the previous day, focusing mostly on the evening party (a pretty traditional thing for most conferences). The photo montage is followed by a gentleman (he doesn't identify himself) who kicks off the keynote by bringing out Seth Vargo, Director of Technical Advocacy at HashiCorp.

Vargo's presentation is titled "The Ecological Impact of Compute," and discusses the environmental impact of cloud computing and the pervasive use of computing/compute power around the world. Vargo presents statistics that show on-premises data centers actually consume more electricity than the mega-scale cloud providers, and that getting these people onto a cloud provider would actually _reduce_ overall power consumption (and, by extension, environmental impacts related to power consumption). Toward the end of Vargo's presentation, it starts to feel more like a sales pitch for Nomad couched in environmental awareness.

At this point, Vargo introduces Kelsey Hightower, Senior Developer Advocate from Google. Hightower's talk is about "Hashinetes," a combination of technologies from Kubernetes and HashiCorp. What is Hashinetes? It's Kubernetes (on a cloud provider) with Consul and Vault on top, and a bit of Nomad mixed in. Why adding Consul and Vault? Consul helps supplement Kubernetes' built-in service discovery across multiple Kubernetes clusters, and Vault offers additional secrets-related features. Mixing in Nomad allows us to handle workloads that aren't necessarily containerized, which is what Kuberrnetes requires.

Hightower fairly quickly moves into a live demo (something he's famous for), and in the demo he shows a three-node Consul cluster running on Kubernetes with a persistent volume claim (to provide persistent storage for Consul). He also shows how to delegate some of Kubernetes' DNS functionality to Consul.

From there, Hightower moves into discussing (and showing) how and why one might want to use Vault in conjunction with Kubernetes. His demonstration shows how Vault can dynamically create (and destroy) credentials.

Next, Hightower shows deploying a Nomad cluster as a StatefulSet on Kubernetes (deploying it via Alexa, I think), and then adds some Nomad worker nodes (running on Google Cloud Platform, Hightower's "on-premises", as he puts it). He then deploys a job onto the Nomad cluster, again using voice recognition via his phone.

At this point, the keynote session wraps up with a quick review of the upcoming talks in the different tracks.
