---
author: slowe
categories: Tutorial
comments: true
date: 2008-12-05T18:11:58Z
slug: installing-the-vi-power-documenter
tags:
- Virtualization
- VMware
title: Installing the VI Power Documenter
url: /2008/12/05/installing-the-vi-power-documenter/
wordpress_id: 1086
---

The VMware Infrastructure Power Documenter (hereafter referred to as VIPD) is a nifty PowerShell script that queries VirtualCenter and produces reports of VM configurations, data center inventory, VM stats, etc. Last time I tried to install VIPD, though, I found the installation instructions seriously lacking. Today I had the opportunity to work with a customer to get this installed, and I wanted to share my information here. These instructions are not to be construed as the "official" way of making VIPD work, just what I had to go through in order for it to work.

First, you'll need to download the prerequisites: Microsoft .NET Framework 3.5, OpenXML Formats SDK, PowerShell 1.0, and the VI Toolkit for PowerShell. As far as I can tell, you can install these in any order you want, other than being sure to install PowerShell before trying to install the VI Toolkit for PowerShell.

Second, copy the VIPD---specifically, the `.PS1` file which is the script itself, the OpenXML PowerTools files, and the DOCX/XLSX formatting templates to a directory on the hard drive. I placed them in the default PowerShell installation directory, but I suppose they could be just about anywhere. The key thing I've found is that the script and the formatting templates need to be in the same directory.

Next, you need to add the OpenXML PowerTools---included with the VIPD download---so that PowerShell can use them. This is where it starts to get dicey. The instructions call for the use of a tool called InstallUtil, but the instructions don't provide any information on _where_ this tool is. I found a version that works in `C:\Windows\Microsoft.NET\Framework\v2.0.50727`. From a command prompt (not PowerShell, as the instructions say), change into that directory and run this command:

	installutil <Full path to OpenXML Power Tools>OpenXml.PowerTools.dll

The instructions said to add another `\OpenXml.PowerTools` to the end of that command, but I couldn't make it work that way.

Finally, you'll need to ensure that PowerShell can run scripts (I believe the command is `Set-ExecutionPolicy RemoteSigned`). Once these steps are done, you should be good to go.

After using VIPD for a short while, I've also noticed that you can't specify a full path for the output file; you can only specify a filename. This means the output file will be created in whatever directory you're in when you run the script.

Anyone who has any additional information or clarification, please feel free to speak up in the comments.
