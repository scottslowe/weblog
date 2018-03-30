---
author: slowe
categories: General
comments: true
date: 2017-08-10T11:30:00Z
tags:
- Writing
- Blogging
- Markdown
- CLI
- AWS
title: Information on the Recent Site Migration
url: /2017/08/10/information-recent-site-migration/
---

Earlier this week, I completed the migration of this site to an entirely new platform, marking the third or fourth platform migration for this site in its 12-year history. Prior to the migration, the site was generated using [Jekyll][link-1] and [GitHub Pages][link-2] following [a previous migration in late 2014][xref-1]. Prior to that, I ran [WordPress][link-3] for about 9 years. So what is it running now?<!--more-->

The site is now generated using [Hugo][link-4], an extraordinarily fast static site generator. I switched to Hugo because it offers a couple of key benefits over Jekyll:

1. Site build times are 10x faster (less than 30 seconds with Hugo compared to over 5 minutes with Jekll)---this directly translates into me being able to test changes to the site much more quickly _(Update: after some optimizations, site build times are down to less than 2 seconds!)_
2. Hugo is a single binary that's easily installed on Linux or macOS (and Windows too, though I don't have any Windows systems)

Hugo also gives me more flexibility that I had with Jekyll, such as generating [lists of articles by tag][link-5] or [lists of articles by category][link-6]. Along with those additions---the ability to browse by tag or category---I've also removed the pagination (I mean, who's _really_ going to page through 188 pages of posts?) and instead made the 50 most recent posts available directly via the home page. The full content of the first 5 are displayed, followed by excerpts/summaries of the next 15 and then links to the next 30 most recent articles. (I'd love to get your feedback on what you think about this arrangement---helpful, not helpful, etc.)

Hugo generates the site from the source code, which is version controlled using Git and [stored on GitHub][link-7]. However, the generated static HTML files are _not_ served by GitHub.

Instead, the generated static HTML files are uploaded to [Amazon S3][link-8]. To streamline the process of uploading the static HTML files to S3, I'm leveraging [a tool called `s3deploy`][link-10], which will only upload files that have actually changed (instead of uploading the entire site every time it is generated, which is what `aws s3 sync` would do).

The act of generating the site (using the `hugo` command) and uploading the changed files (using `s3deploy`) are bundled together in a `Makefile` that allows me to easily build the site and upload the changed files with a single command.

Although it's certainly possible to serve a static site out of an S3 bucket, this site is instead served by the [Amazon CloudFront][link-9] content distribution network (CDN). Right now, the CloudFront distribution is limited to only using the US, Canada, and Europe endpoints until I get a better feel for utilization and cost. Assuming the costs aren't too high, I can easily expand that to include CDN endpoints worldwide, allowing me to provide better latency to readers all around the globe. Path invalidation (to refresh the CDN cache) using `aws cloudfront create-invalidation` isn't yet automated, but I'm working on that aspect.

Over the next few weeks, I'll be fine-tuning the SEO of the site and making other adjustments to optimize the site for S3 and CloudFront. Those changes should be pretty much invisible to most readers.

Hopefully, this helps answer some questions about why I migrated the site (again), and what is currently in use behind the scenes to power the site. If anyone has additional questions, feel free to [hit me up on Twitter][link-11]. Thanks for reading!



[link-1]: http://jekyllrb.com/
[link-2]: https://pages.github.com/
[link-3]: https://wordpress.org/
[link-4]: https://gohugo.io/
[link-5]: /tags/
[link-6]: /categories/
[link-7]: https://github.com/scottslowe/weblog/
[link-8]: https://aws.amazon.com/s3/
[link-9]: https://aws.amazon.com/cloudfront/
[link-10]: https://github.com/bep/s3deploy
[link-11]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2015-01-05-blog-migration-complete.md" >}}
