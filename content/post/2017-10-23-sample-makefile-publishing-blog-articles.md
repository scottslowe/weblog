---
author: slowe
categories: Explanation
comments: true
date: 2017-10-23T12:00:00Z
tags:
- Writing
- Blogging
- CLI
- Productivity
title: A Sample Makefile for Publishing Blog Articles
url: /2017/10/23/sample-makefile-publishing-blog-articles/
---

As some readers may already know, this site has been running on a static site generator since late 2014/early 2015, when [I migrated from WordPress to Jekyll on GitHub Pages][xref-2]. I've since migrated again, [this time to Hugo on S3/CloudFront][xref-2]. Along the way, I've taken an interest in using `make` and a `Makefile` to help automate certain tasks at the CLI. In this post, I'll share how I'm using a `Makefile` to help with publishing blog articles.<!--more-->

If you're not familiar with `make` or its use of a `Makefile`, have a look at this article I wrote on [using a `Makefile` with Markdown documents][xref-3], then come back here.

In general, the process for publishing a blog post using Hugo and S3/CloudFront basically looks like this:

1. Write the blog post. (I haven't found a tool to automate this yet!)
2. Put the blog post into the right section of the Hugo directory tree. (In my setup, it's in the `content/post` directory.)
3. Build the static site using `hugo`.
4. Upload the resulting HTML files to the appropriate S3 bucket.
5. Invalidate the appropriate URLs (paths) in AWS CloudFront so that the CDN picks up the new files/pages.

Some of these steps are easy to automate; what I've tackled with `make` and a `Makefile` are steps 3, 4, and 5. Here's the current version of the `Makefile` I'm using (with a few randomizations to protect some of my information):

```
SRC_DIR := public/
CF_DIST := ABCDEFG1234567
BUCKET := my-destination-bucket

build:
    @hugo

upload:
    @s3deploy -source=$(SRC_DIR) -region=us-east-1 -bucket=$(BUCKET)

invalidate:
    @aws cloudfront create-invalidation --distribution $(CF_DIST) \
    --paths / /archives/ /categories/ /feed.xml

deploy: build upload invalidate

server:
    -@hugo server

clean:
    @rm -rf $(SRC_DIR)
```

With this `Makefile` in place, _my_ process for publishing looks like this:

1. Write the blog post in Markdown.
2. Move the blog post's Markdown file into the appropriate directory (in my case, `content/post`).
3. Run `make build` to verify that the site builds correctly. If it runs into errors, fix the errors and repeat until the site builds correctly.
4. Run `make deploy`. This will build the site (which I've verified will build correctly in the previous step), upload the files to S3 using `s3deploy`, and then invalidate a few paths. Done!

In earlier versions of the `Makefile`, I didn't have all the different targets ("build", "upload", and "invalidate") broken out, but after working with this approach for a while it made more sense to me to split out the steps, then list them as dependencies in a final "deploy" target. So far, this approach has worked out pretty well.

One thing I haven't quite worked out yet is the "server" target, which simply runs `hugo server` (for serving up a local version of the site). The hyphen at the front of this command tells `make` to ignore errors returned by the command it runs (so that it will ignore the error produced when I press Ctrl-C to stop Hugo), but this doesn't work so well. If anyone has any suggestions to work around this, I'm open to hearing them (hit [me on Twitter][link-1]).

In any case, in the event any readers out there are considering the use of a static site generator, I hope this information on helping to streamline and automate the process of publishing a post using a `Makefile` proves helpful.



[link-1]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2017-08-10-information-recent-site-migration.md" >}}
[xref-2]: {{< relref "2015-01-05-blog-migration-complete.md" >}}
[xref-3]: {{< relref "2017-05-08-using-makefile-with-markdown-documents.md" >}}
