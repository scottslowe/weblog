---
author: slowe
categories: Information
comments: true
date: 2017-01-16T00:00:00Z
tags:
- Linux
- CLI
- Markdown
title: OPML-to-Markdown Conversion Script
url: /2017/01/16/opml-to-markdown-script/
---

In this post, I'd like to share a script I wrote to help with converting Outline Processor Markup Language (OPML) documents to Markdown. If you read the recent [update on my Linux migration plans][xref-2], you may recall that I identified OPML files (created in OmniOutliner) as an area where some work was going to be required. This script is the result of my efforts in this area.

&lt;aside&gt;Before I continue, I want to very briefly point out that this script was written to help in _my_ specific use case. It's quite likely that you'll want or need to adjust the behaviors of this script in order to meet the needs of your particular use case.&lt;/aside&gt;

This script takes advantage of two tools: `pandoc` and `sed`. `pandoc` is [a third-party tool][link-1] that is easily installed on Ubuntu using `apt` or `apt-get`. (I haven't checked other Linux distributions, but I suspect packages are available there as well.) `pandoc` is also available for OS X, making it a very handy cross-platform tool to have in my toolchest. (See [this post][xref-1] for more information on how you can use `pandoc` in a Markdown-heavy environment.) `sed`, of course, is a very well-known tool that is almost guaranteed to be available on just about any Linux distribution (and other UNIX-like OSes, including OS X). If you're interested in using this script, you'll want to first ensure you have these two tools present on your system.

The script has two basic parts:

1. A `pandoc` command converts the OPML to MultiMarkdown. The command looks something like this:

        pandoc --from=opml --to=markdown_mmd --atx-headers

    The result of the command is sent to STDOUT.

2. The output of the `pandoc` command is then piped through a series of `sed` commands to turn level 2 and higher headings into MultiMarkdown bulleted lists, finally being redirected into the final destination file.

You can see the entire script in [my GitHub "scripts" repository][link-2], where I've been collecting various scripts I've written. So far, I've tested this on a number of my OPML files, and it seems to work well. However, if you take a look at the script and see a better way of handling things, feel free to fork the repository, make your changes, and submit a pull request.

## Why This Script

I'd like to briefly talk about why I needed this script. See, I use OPML as an outline that includes data, not just headings. Most OPML-to-Markdown conversion tools, including `pandoc`, simply assume that each line in an OPML file should be a heading. For me (and this may vary for others), any second-level or higher entries in the OPML file are data, _not_ headings. Thus, I use `sed` and regular expressions to convert second-level and third-level headings into bulleted lists.

Thanks!



[link-1]: http://pandoc.org/
[link-2]: https://github.com/scottslowe/scripts
[xref-1]: {{< relref "2014-05-19-some-useful-markdown-tools-for-os-x.md" >}}
[xref-2]: {{< relref "2016-12-16-linux-migration-initial-progress-report.md" >}}
