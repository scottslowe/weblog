---
author: slowe
categories: News
comments: true
date: 2011-12-15T23:00:17Z
slug: uimoperations-3-0-now-ga
tags:
- Networking
- Storage
- Vblock
- Virtualization
title: UIM/Operations 3.0 Now GA
url: /2011/12/15/uimoperations-3-0-now-ga/
wordpress_id: 2502
---

Within the last couple of days, I received an e-mail notification that UIM/Operations 3.0 had been finalized and was now generally available (i.e., it was now considered GA).

For those that aren't familiar, UIM has two flavors:

* UIM/Provisioning (also referred to as UIM/P), which is tasked with handling provisioning/de-provisioning tasks in a Vblock. This would include tasks like deploying UCS B-series blades, zoning FC fabrics, and setting up storage pools.

* UIM/Operations (also referred to as UIM/O) is tasked with providing near real-time visibility into the Vblock, as well as root cause and impact analysis.

In addition to support for UIM/P 3.0 (more info [here][1]) and all associated Vblock types, this latest release of UIM/O adds the following features:

* Model-based deterministic automated root cause analysis for faults in a Vblock environment

* Automated impact analysis that visualizes impact on higher-order abstractions such as vApps, UIM Services (these are defined within UIM/P) and Vblocks

* Event forwarding via SNMP traps to enable northbound integration

* Automation of trap reception from MDS and Nexus switches

* Saving and restoring user preferences

As with UIM/P, the new version of UIM/O is available to authorized users on Powerlink:

Home > Support > Software Downloads and Licensing > Downloads E-I > Ionix Unified Infrastructure Manager/Operations

Documentation for UIM/O 3.0 is also available on Powerlink:

Home > Support > Technical Documentation and Advisories > Software ~ E-I ~ Documentation > Ionix Family > Ionix for Data Center Automation and Compliance > Ionix Unified Infrastructure Manager/Operations > 3.0 and Service Packs

(Think that's a deep enough structure to navigate?)

Enjoy!

[1]: {{< relref "2011-11-09-new-version-of-uimp.md" >}}
