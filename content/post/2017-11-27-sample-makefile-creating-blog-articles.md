---
author: slowe
categories: Explanation
comments: true
date: 2017-11-27T12:00:00Z
tags:
- Writing
- Blogging
- CLI
- Productivity
title: A Sample Makefile for Creating Blog Articles
url: /2017/11/27/sample-makefile-creating-blog-articles/
---

In October of this year, I published a blog post talking about [a sample `Makefile` for publishing blog articles][xref-1]. That post focused on the use of `make` and a `Makefile` for automating the process of a publishing a blog post. This post is a companion to that post, and focuses on the use of a `Makefile` for automating the creation of blog posts.<!--more-->

Since early 2015, this site has been running as a static site with the content created using Markdown. In its first iteration as a static site, the HTML was generated using [Jekyll][link-4] and hosted on [GitHub Pages][link-5]. In the current iteration, the HTML is generated using [Hugo][link-3], hosted on [Amazon S3][link-1], and served via [Amazon CloudFront][link-2]. In both cases, the use of Markdown as the content format also required specific front-matter to instruct the static site generator how to create the HTML. For Hugo, the front-matter looks something like this (I use YAML, but other formats are supported):

```yaml
---
author: slowe
categories: Explanation
date: 2017-11-27T12:00:00Z
tags:
- Writing
- Blogging
- Productivity
title: Sample Blog Post title
url: /2017/11/27/sample-blog-post-title/
---
```

There are obviously a lot of different ways to automate the creation of this front-matter (text expansion utilities, text editor snippets, macros, etc.). I chose to tackle this using a `Makefile` so that I had some level of independence from the specific editor or operating system I was using (`make` is easily accessible on both macOS and Linux). After some trial and error, I finally arrived at this `Makefile`:

```
POST_TEMPLATE := ~/Templates/blog-post-template.md
editor ?= /usr/local/bin/subl -n
name ?= new-blog-post
offset ?= +0d
DDATE := $(shell date -v $(offset) +%Y-%m-%d)
SDATE := $(shell date -v $(offset) +%Y/%m/%d)

new:
    @cat $(POST_TEMPLATE) | \
    sed "s/YYYY-MM-DD/$(DDATE)/g" | \
    sed "s|YYYY/MM/DD|$(SDATE)|g" | \
    sed "s|/title|/$(name)/|g" >> $(DDATE)-$(name).md
    @$(editor) $(DDATE)-$(name).md
```

I'm no expert with `make`, so there may be ways to optimize this (feel free to [hit me up on Twitter][link-6] with suggestions). This `Makefile` leverages two user-supplied values (default values are supplied in case the user doesn't provide these values on the command line):

* The first is `name`, which is used for both the filename and in the "url" value in the front-matter
* The second is `offset`, which is an offset (typically in days, like "+1d") from the current date (more on this in a moment)

With these two values, what the "new" target in this `Makefile` does is this:

1. Sends the content of the `$(POST_TEMPLATE)` variable to STDOUT.
2. STDOUT is piped into the the first `sed` command, which replaces the templated "YYYY-MM-DD" value for the "date" YAML front-matter with the current date, adjusted by the user-supplied `offset` variable (which is set to "+0d" if the user doesn't supply a value). This date value is taken from the `DDATE` variable defined at the top of the file.
3. The output of the first `sed` command is piped into the second `sed` command, which performs the same date manipulation, but this time on the templated value of the "url" YAML front-matter entry. (The value is taken from the `SDATE` variable defined at the top of the file.)
4. The output of the second `sed` command is piped into the third `sed` command, which replaces the final section of the templated "url" front-matter with the value of the `name` variable passed in by the user (or the default value, if the user doesn't supply a value).
5. The final content is redirected into a file whose filename is constructed from the date (taken from the `DDATE` variable) and the value of `name`.

Using this `Makefile` is pretty straightforward:

* If I type `make` or `make new` (no additional values), then it will take the current date and the default value of "new-blog-post". For example, if I typed this today (Monday, 27 November), I'd get a filename of `2017-11-27-new-blog-post.md`, and the YAML front-matter would have the appropriate values inserted. (In case you weren't aware, typing just `make` works because "new" is the first target.)
* If I type `make new name=specific-name-for-post`, then the resulting file generated with have the current date in the content and the filename, but the rest of the filename and the "url" YAML front-matter will use the value of `name` as provided on the command line. If I typed this today, I'd get a filename of `2017-11-27-specific-name-for-post.md`.
* If I type `make new name=another-specific-name offset=+1d`, then the generated filename will use the value of `name` (as will the "url" YAML front-matter entry), but the date will be one day in the future---as specified by the value of `offset`. If I typed this today, the file generated would be named `2017-11-28-another-specific-name.md`.

In all these examples, the file is immediately opened in [Sublime Text][link-7], as instructed by the default value of `editor`. If I wanted to change that, I can just type `make new name=a-third-specific-name editor=atom` and it would open in [Atom][link-8] instead. This provides some additional flexibility if needed.

I'll continue to refine this `Makefile` as I learn more, and readers are encouraged to [hit me on Twitter][link-6] with suggestions for improvement. I hope this information proves useful to other bloggers using static site generators.



[link-1]: https://aws.amazon.com/s3/
[link-2]: https://aws.amazon.com/cloudfront/
[link-3]: http://gohugo.io/
[link-4]: https://jekyllrb.com/
[link-5]: https://pages.github.com/
[link-6]: https://twitter.com/scott_lowe
[link-7]: http://www.sublimetext.com/
[link-8]: https://atom.io/
[xref-1]: {{< relref "2017-10-23-sample-makefile-publishing-blog-articles.md" >}}
