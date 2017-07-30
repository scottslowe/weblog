---
author: slowe
categories: Liveblog
comments: true
date: 2012-09-12T18:13:39Z
slug: dats002-big-data-meets-high-performance-computing
tags:
- IDF2012
- Intel
title: 'DATS002: Big Data Meets High Performance Computing'
url: /2012/09/12/dats002-big-data-meets-high-performance-computing/
wordpress_id: 2853
---

This is session DATS002, titled "Big Data Meets High Performance Computing." The speakers are John Hengeveld from Intel and Micahel Franklin from UC Berkeley. The title sounds like this might be a marketing session, but I'm hoping that this material will be substantial instead of just fluff.

Hengeveld starts the session by reminding attendees that Big Data is really about the insights that are gained from the data. Big Data isn't about the data, it's about the insight and understanding. In this context, discussions of Big Data that talk about millions of Facebook posts, or data posted to Google or Twitter, or clickstream data, often miss the opportunity to discuss the insight that can be gained from the data. Hengeveld describes Big Data as an oilfield---massive, deep, full of "rich" information. Big data technologies are the "mining" technologies that allow you to extract value from that oilfield.

Continuing the oilfield analogy, you need something that can work with the data to produce the insight---something that can refine insight from the raw data. This is HPC---high-performance computing. So what kinds of insights can HPC mine from Big Data? Better medical therapies, improved security through facial recognition, analyzing routes through traffic based on cell phone density, and urban planning and simulation are four examples that Hengeveld shares in the session.

At this point the presentation shifts to Professor Franklin from UC Berkeley, who works at the AMP Lab and is doing some Big Data research. Franklin describes "AMP" as standing for "Algorithms, Machines, & People"---meaning that all three have value in extracting value from Big Data. AMP was launched in February 2011 and is funded by a consortium of organizations, including Intel (hence the connection to IDF). All of the software that AMP is generating is released under the BSD license.

Franklin takes a moment to point out that much of the data that comprises Big Data is generated from online activities, but an even greater amount of data comes from tracking online activities. This could be from systems that track the user experience, logs from systems that support the online activities, health/utilization reports from the underlying infrastructure, etc.

What AMP is building is called BDAS, which stands for Berkeley Data Analysis System. In addition to compute resources, BDAS is also designed to leverage people (crowdsourcing) and data collectors such as public data sources. The goals of BDAS are threefold: 1) to effectively manage cluster resources; 2) efficiently extract value out of big data; and 3) continuously optimize cost, time, and answer quality. In order to support these initiatives, AMP has pushed data quality and answer quality attributes deep into the system. The BDAS components that have been released so far includes Mesos (the cluster resource management layer); Spark (an alternative to Hadoop that is targeted at interactive and iterative workloads); and Shark (a port of Hive from Hadoop to Shark; completely compatible with existing Hive queries). Mesos is under the governance of the Apache Foundation; the other projects are available on Github (Shark is [here](https://github.com/amplab/shark); Spark is [here](https://github.com/mesos/spark)). These are licensed with the BSD license, as mentioned earlier.

Franklin next goes over an application AMP developed called Carat. Carat uses crowdsourced data in conjunction with AWS and Spark to do analysis of applications and battery usage. This helps determine correlations between battery life and application usage. Free iOS and Android apps are available to help contribute to Carat (and gain information from Carat, such as which applications drain your battery most).

At this point, Hengeveld takes the platform again to wrap up the session. He reiterates the need for new big data software stacks (like Spark and Shark) to keep up with growing data volume, velocity, and variety (the "three V's of Big Data"). The discussion then shifts to a review of the technologies and initiatives that Intel is developing to help in the Big Data and HPC (and Big Data+HPC together). One of the areas that Intel is working on is fixing "1960's era" storage hierarchies that simply don't support Big Data paradigms.  The new approach is object-based storage; Hengeveld uses Lustre as an example. Another area of development is interconnect technology; so Intel is working to improve effective bandwidth by lowering latency with Intel True Scale (formerly Qlogic) technologies. Finally, Hengeveld believes that the Intel Many Integrated Core (MIC) architecture, now officially known as Xeon Phi, will really help with processing highly parallel data structures.

At this point, Hengeveld wraps up with a call to action for developers and opens the floor to questions.
