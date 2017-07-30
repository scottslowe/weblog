---
author: adelp
categories: Education
comments: true
date: 2009-04-07T11:16:10Z
excerpt: This post provides an introduction to some of the technical details surrounding
  Intel's new Nehalem CPUs.
slug: introduction-to-nehalem-cpus
tags:
- Hardware
- Intel
- Nehalem
- GuestPost
title: Introduction to "Nehalem" CPUs
url: /2009/04/07/introduction-to-nehalem-cpus/
wordpress_id: 1272
---

_By Aaron Delp_

Hello everyone! It's Aaron again. Im sorry for falling off the radar for a bit. A new generation of Intel processors is upon us and I felt the need to come out of seclusion to share some recent findings regarding the new architecture. Todays article will explore the new processor offerings. I will be following this up with one (or more depending on the length) about the memory architecture and interconnects.

There is one simple reason why I wrote this article. You can no longer pick a processor based on clock speed. The Nehalem processors have levels now and each level provides additional features and functionality lacking in the lower levels. You will need to be careful when choosing a processor if you are looking for certain features. Here is a quick table listing the models and the features:

| CPU and Speed   | Watts | Max Mem Speed | Turbo Mode/Hyper-Threading |
|-----------------|-------|---------------|----------------------------|
| X5570 (2.93GHz) | 95W   | 1333 MHz      | Yes                        |
| X5560 (2.80GHz) | 95W   | 1333 MHz      | Yes                        |
| X5550 (2.66GHz) | 95W   | 1333 MHz      | Yes                        |
| E5540 (2.53GHz) | 80W   | 1066 MHz      | Yes                        |
| E5530 (2.40GHz) | 80W   | 1066 MHz      | Yes                        |
| E5520 (2.26GHz) | 80W   | 1066 MHz      | Yes                        |
| E5506 (2.13GHz) | 80W   | 800 MHz       | No                         |
| E5504 (2.00GHz) | 80W   | 800 MHz       | No                         |
| E5502 (1.66GHz) | 80W   | 800 MHz       | No                         |

I find the Max Memory Speed particularly interesting. As you will see in the next article, memory speed can get pretty complex very quickly. The more memory that is installed in the system, the lower the clock speed on the memory. The days of installing in matched pairs and forgetting about it are gone.

What is Turbo Mode and Hyper-Threading you ask? Hyper-Threading as far as I can tell (please leave feedback if this incorrect!) is the same old Hyper-Threading we knew and loved from past chipsets. Turbo mode is interesting though. Think of it as Burst Mode for processors. If your OS supports it, the CPU will increase the clock speed as long as you are within the thermal/power thresholds for the chip. The ability to go into Turbo mode depends on the number of active cores. If you are using most of the cores, the chip will be less likely to go into Turbo mode.

UPDATE: Keith from Intel has provided a great explanation of Turbo mode from a hardware perspective in the comments section. I wanted to include it here as a direct quote. Thanks Keith!

>Turbo mode is mostly independent of OS support. On CPUs that support Turbo, it is implemented as the P0 p-state in the CPU. It looks & smells like a CPU that is simply running in the highest-frequency P-state. The PCU (power control unit) in Nehalem handles the rest.
