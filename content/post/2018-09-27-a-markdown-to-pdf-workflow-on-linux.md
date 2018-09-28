---
author: slowe
categories: Explanation
comments: true
date: 2018-09-27T12:00:00Z
tags:
- Linux
- CLI
- Markdown
- Fedora
- HTML
title: A Markdown-to-PDF Workflow on Linux
url: /2018/09/27/a-markdown-to-pdf-workflow-on-linux/
---

In May of last year I wrote about [using a `Makefile` with Markdown documents][xref-1], in which I described how I use `make` and a `Makefile` along with CLI tools like `multimarkdown` (the binary, not the format) and [Pandoc][link-1]. At that time, I'd figured out how to use combinations of the various CLI tools to create various formats from the source [Markdown][link-5] document. The one format I hadn't gotten right at that time was PDF. Pandoc _can_ create PDFs, but only if [LaTeX][link-2] is installed. This article describes a method I found that allows me to create PDFs from my Markdown documents _without_ using LaTeX.<!--more-->

Two tools are involved in this new conversion process: Pandoc, which I've discussed on this site before; and [`wkhtmltopdf`][link-4], a new tool I just recently discovered. Basically, I use Pandoc to go from Markdown (MultiMarkdown, specifically) to HTML, and then use `wkhtmltopdf` to generate a PDF file from the HTML.

The first step in the process is to use Pandoc to convert from Markdown to HTML, including the use of CSS to include custom formatting. The command looks something like this:

    pandoc --from=markdown_mmd+yaml_metadata_block+smart --standalone \
    --to=html -V css=/home/slowe/Documents/std-styles.css \
    --output=<destination-html-filename> <source-md-filename>

This generates an HTML document and links it to the specified CSS document for formatting. I _could_ embed the CSS specification in the YAML metadata block at the top of the Markdown document, but then I'd lose the ability to create multiple versions of the document with different formatting. The `-V css=` parameter lets me specify formatting instructions at the time of creating the HTML document.

Once I have the HTML document, then I can create a paginated PDF with this command:

    wkhtmltopdf -B 25mm -T 25mm -L 25mm -R 25mm \
    -q -s Letter <source-html-filename> <destination-pdf-filename>

This will create a paginated PDF, based on US Letter paper size, with 25mm margins all the way around.

Putting these together into a `make` workflow, I can do something like this:

```
PD = /usr/local/bin/pandoc
PDFLAGS = --from=markdown_mmd+yaml_metadata_block+smart --standalone
WK = /usr/bin/wkhtmltopdf
WKFLAGS = -B 25mm -T 25mm -L 25mm -R 25mm -q -s Letter
CSS ?= /home/slowe/Documents/std-styles.css

%.md.pdf: %.md
        $(PD) $(PDFLAGS) --to=html -V css=$(CSS) $< | $(WK) $(WKFLAGS) - $@
```

With this in place, all I have to do is type `make filename.md.pdf`. Make will use `pandoc` to convert from Markdown to HTML, then pipe those results to `wkhtmltopdf` to create a paginated PDF. The beauty of defining the stylesheet to use as a variable is that I can override it on the command line if desired:

    make CSS=/path/to/alternate/styles.css filename.md.pdf

This makes it super-easy to create PDFs from source Markdown documents.

Have suggestions for improvement? I'm always open to learn more---feel free to [hit me on Twitter][link-5].

[link-1]: http://pandoc.org/
[link-2]: https://www.latex-project.org/
[link-3]: https://wkhtmltopdf.org/
[link-4]: https://daringfireball.net/projects/markdown/
[link-5]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2017-05-08-using-makefile-with-markdown-documents.md" >}}
