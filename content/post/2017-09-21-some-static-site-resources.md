---
author: slowe
categories: Information
comments: true
date: 2017-09-21T16:00:00Z
tags:
- Blogging
- CLI
- AWS
title: Some Static Site Resources
url: /2017/09/21/some-static-site-resources/
---

Over the last few days---prompted perhaps by [my article with some additional information][xref-1] on my site migration---a few folks in the community have reached out to me to share some resources they thought I might find useful. In turn, I'd like to share them with you, my readers, in the event you might find them useful as well.<!--more-->

This is (clearly and obviously) not a comprehensive list, but here's what folks have shared with me over the last few days:

* Josh Habdas shared [this link][link-1] with me; it's a write-up he did that involves the use of a Ruby-based tool called `s3_website`. The main problem I have with this write-up is that it hides too many of the details, preventing (in my opinion) some of the valuable learning that can come from such an effort.
* [This article][link-2] by Ricardo Feliciano of CircleCI _does_ expose some of the gory details, and might be useful for those considering the inclusion of a CI/CD pipeline in their blogging workflow (like I am).
* Finally, I found [this post][link-3] describing how to build a multi-region S3+CloudFront setup that would protect your site in the event of a single S3 region being unavailable.

I'll update this post with additional resources as I find them (or as folks share them with me). I hope these are helpful!



[link-1]: https://habd.as/zero-to-http-2-aws-hugo/
[link-2]: https://circleci.com/blog/build-test-deploy-hugo-sites/
[link-3]: https://static-site.jolexa.us/?__s=smmxtq8pks5yjd519tws
[xref-1]: {{< relref "2017-09-18-some-qa-about-migration-hugo.md" >}}
