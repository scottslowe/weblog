---
author: slowe
categories: Explanation
comments: true
date: 2017-05-08T00:00:00Z
tags:
- Linux
- CLI
- Markdown
- Writing
title: Using a Makefile with Markdown Documents
url: /2017/05/08/using-makefile-with-markdown-documents/
---

It's no secret that I'm a big fan of using [Markdown][link-1] (specifically, [MultiMarkdown][link-2]) for the vast majority of all the text-based content that I create. Over the last few years, I've created used various tools and created scripts to help "reduce the friction" involved with outputting Markdown source files into a variety of destination formats (HTML, RTF, or DOCX, for example). Recently, [thanks to Cody Bunch][link-3], I was pointed toward the use of a `Makefile` to assist in this area. After a short period of experimentation, I'm finding that I really like this workflow, and I wanted to share some details here with my readers.<!--more-->

First, if you're not familiar with `make` and its use of a `Makefile`, check out [this introduction][link-4]. There's a _ton_ of power and flexibility here, of which I've only scratched the surface so far. The basic gist behind a `Makefile` is that it provides a set of instructions to the `make` command. Each set of instructions is tied to a target, which has one or more dependencies. In the "traditional" use cases for `make`, this is to allow programmers to define how a set of files should be compiled as well as the dependencies upon which those files relied.

In this use case, we'll re-purpose the `Makefile` functionality to define how we go about creating a particular output format based on a source Markdown document. For example, the following rule defines how to build the file `report.html`, which has as a dependency the source file `report.md`:

    report.html: report.md
        multimarkdown --to=html --full --smart $< >> $@

There's a few weird things here that may deserve some explanation:

* The `$<` represents the source file on which `make` is currently operating. In this particular rule, this means `report.md`.

* The `$@` represents the target file on which `make` is currently operating. So, for this rule, it represents `report.html`.

When you run `make report.html`, Make will first check to see if the dependencies for the target (`report.html` is the target) are newer than the target itself. If this is the case, that means Make needs to update the target. If the target doesn't exist, that means Make needs to create the target. How does Make create or update the target? Using the commands under the rule---so, running the `multimarkdown` binary with the specified command line parameters and outputting the result to the target (which, in this case, creates an HTML file from the source Markdown file).

Naturally, defining the source and destination file name every time you create a new Markdown file isn't ideal, so we can take this idea and make it more generic through the use of wildcards. With this one change to the earlier rule, we've now make it possible to create an HTML file from _any_ source Markdown file:

    %.html: %.md
        multimarkdown --to=html --smart $< >> $@

By extending this example to other formats, I can create easily create a set of rules for generating any number of output formats from a single Markdown document. Here's an example set of rules:

    %.html: %.md
        multimarkdown --to=html --smart $< >> $@

    %.rtf: %.md
        pandoc --from=markdown --to=rtf --smart --standalone --output=$@ $<

    %.docx: %.md
        pandoc --from=markdown --to=docx --smart --standalone --output=$@ $<

Using an approach like this makes it really easy to generate a variety of output formats. If I need to generate a Rich Text Format (RTF) document from a Markdown source file, I just run `make <filename>.rtf`. Make will find the matching Markdown source file and---using `pandoc`---create the RTF file. If the target already exists and the Markdown file hasn't been updated (based on file modification date), then Make will do nothing. Likewise, if I need to create a DOCX (Microsoft Word) file, I can just run `make <filename>.docx`. It's pretty straightforward, to be honest.

As I mentioned earlier, Make is _so_ much more powerful than what I've shown you here (or even what I'm using at the moment). Currently, I'm exploring the use of Make to help streamline the creation of new documents; stay tuned for more details after I've worked through a few of the details.

Until then, if you're looking for a new way to handle output formats for your Markdown documents, I'd encourage you to investigate whether using Make and a `Makefile` will work for you.



[link-1]: http://daringfireball.com/markdown/
[link-2]: http://fletcherpenney.net/multimarkdown/
[link-3]: http://blog.codybunch.com/2017/04/17/On-Computerized-Note-Taking/
[link-4]: https://www.digitalocean.com/community/tutorials/how-to-use-makefiles-to-automate-repetitive-tasks-on-an-ubuntu-vps
