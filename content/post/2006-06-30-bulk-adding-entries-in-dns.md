---
author: slowe
categories: Tutorial
comments: false
date: 2006-06-30T22:58:26Z
slug: bulk-adding-entries-in-dns
tags:
- Microsoft
- Windows
title: Bulk Adding Entries in DNS
url: /2006/06/30/bulk-adding-entries-in-dns/
wordpress_id: 286
---

While we are on the topic of bulk changes---I've discussed [bulk changes in Active Directory][1] and [bulk adds to a WINS server][2]---I thought I might discuss bulk adds of resource records to a DNS zone.

There are two pieces to the formula for bulk adding records to a DNS zone. First, we'll reuse the `for /f` command used in automating the addition of entries into WINS. This time we'll couple it with the `dnscmd.exe` tool from the Windows Server 2003 Support Tools ([this link](http://www.microsoft.com/downloads/info.aspx?na=22&p=5&SrcDisplayLang=en&SrcCategoryId=&SrcFamilyId=&u=%2fdownloads%2fdetails.aspx%3fFamilyID%3d6ec50b78-8be1-4e81-b3be-4e7ac4f0912d%26DisplayLang%3den) is for the 32-bit SP1 tools).

The format of the `dnscmd.exe` tool to add a record in DNS is:

	dnscmd <server> /RecordAdd <zone> <node> <RR type> <RR data>

To create a host (A) record, the actual command would look like this:

	dnscmd . /RecordAdd example.net host1 A 10.20.30.40

The "." syntax here refers to the local server; this can be easily substituted with an IP address or hostname of a remote DNS server.

Using some of the same techniques as before, combining `dsncmd.exe` with the `for` batch command allows us to do something like this:

```text
for /f "tokens=1,2" %1 in (newhosts.txt) do 
@dnscmd dns.example.net /RecordAdd example.net %1 A %2
```

This assumes that the "newhosts.txt" file contains something like this:

	host1 10.20.30.40  
	host2 11.22.33.44  
	host3 12.24.36.48

Here's a small twist, though: What if your list isn't space delimited, but comma delimited? No problem, just adjust your command accordingly:

```text
for /f "tokens=1,2 delims=," %1 in (newhosts.txt) do 
@dnscmd dns.example.net /RecordAdd example.net %1 A %2
```

The "delims=," parameter tells the `for` command to use a comma as the delimiter, allowing us to use comma-separated input.

With this command, we now can pretty easily add large numbers of hosts to a DNS zone file.

[1]: {{< relref "2006-06-20-mass-changes-in-active-directory.md" >}}
[2]: {{< relref "2006-06-28-automating-static-entries-in-wins.md" >}}
