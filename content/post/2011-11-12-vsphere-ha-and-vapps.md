---
author: slowe
categories: Explanation
comments: true
date: 2011-11-12T13:45:47Z
slug: vsphere-ha-and-vapps
tags:
- Virtualization
- VMware
- vSphere
title: vSphere HA and vApps
url: /2011/11/12/vsphere-ha-and-vapps/
wordpress_id: 2463
---

This is probably obvious to most everyone, but for those who don't already know it's important to point this out: vSphere HA does **not** honor vApp startup settings. So while you can go into the properties for a vApp and define the order in which VMs will start (or stop) and the timing between those actions, vSphere HA will not recognize or honor those settings. Therefore, in a situation where an ESX/ESXi host fails and that host is running a vApp, the individual VMs in the vApp could be restarted in an entirely different order than what you specified in the vApp settings.

Be sure to plan accordingly when using vApps in your environment.
