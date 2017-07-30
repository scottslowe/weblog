---
author: slowe
categories: Information
comments: false
date: 2006-07-19T11:08:36Z
slug: gpmc-scripts
tags:
- ActiveDirectory
- Microsoft
title: GPMC Scripts
url: /2006/07/19/gpmc-scripts/
wordpress_id: 303
---

These scripts are mostly VBScript, with a couple JScript, and are (by default) found in the `Program Files\GPMC\Scripts` folder. They are designed to be executed with the `cscript.exe` command-line script interpreter, and they all offer help via the `/?` parameter on the command line.

Here's a quick look at three scripts that are (in my opinion) particularly useful:

* _FindGPOsByPolicyExtension.wsf:_ This script finds all the GPOs that contain settings in a particular client-side extension (CSE), say "Software Installation." This is pretty helpful when you've inherited an existing GPO infrastructure and you have no idea how the existing GPOs play together. With this script, you can easily find GPOs that contain overlapping settings.

* _FindUnlinkedGPOs.wsf:_ This script, as the name implies, produces a list of GPOs that are defined but not linked to a scope of management (SoM, such as a domain, OU, or site). Sure, you could do this in the graphical GPMC console, but when you've got a large number of SoMs and/or a large number of GPOs, that could take some time. This script does the hard work for you.

* _ListSOMPolicyTree.wsf:_ This script produces a textual listing of all the SoMs (scope of management, i.e., a domain, site, or OU) and the GPOs that are linked to each SoM. This makes it easier to try to determine which GPOs are being applied to an object, and it's handy to include this information in documentation.

* _GPOsWithNoSecurityFiltering.wsf:_ Here's another handy script. This script finds GPOs that have no "Apply" permission to any security principals. The GPOs are enabled, the GPOs are linked, but because they lack the "Apply" permission they won't affect anyone or anything.

There are so many more scripts that I haven't even touched on here that are equally useful for backing up GPOs, exporting GPOs, copying GPOs between domains, etc.

Keep in mind that you could easily schedule these scripts to run on a regular basis to help keep your documentation up to date and complete.

Visit [this page](http://technet2.microsoft.com/WindowsServer/en/Library/438bf488-dafe-466d-8fdc-cf847946cf5c1033.mspx?mfr=true) for more complete details on all the scripts and their purpose.
