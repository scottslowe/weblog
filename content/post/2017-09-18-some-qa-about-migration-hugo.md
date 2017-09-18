---
author: slowe
categories: Explanation
comments: true
date: 2017-09-18T12:00:00Z
tags:
- Writing
- Blogging
- CLI
- AWS
title: 'Some Q&A About the Migration to Hugo'
url: /2017/09/18/some-qa-about-migration-hugo/
---

As you may already know, I recently completed the migration of this site from [GitHub Pages][link-1] (generated using [Jekyll][link-2]) to S3/CloudFront and [Hugo][link-3] for static site generation. Since then, I've talked with a few readers who had additional questions about the site migration. I thought others might have the same questions, so I decided to gather the most common questions here and share the answers with everyone.<!--more-->

(For those who need a quick primer on how the site is set up/served, refer to [this post][xref-1].)

I'll structure the rest of this post in a "question-and-answer" format.

_**Q:** Why migrate away from Jekyll?_

**A:** Some of this is tied up with GitHub Pages (see the next question), but the key things that drove me away were very slow build times (in excess of five minutes), limited troubleshooting, dealing with Ruby dependencies in order to run local Jekyll builds (needed to help with troubleshooting), and limited functionality (due in part to GitHub Pages' restrictive support for plugins).

_**Q:** Why migrate away from GitHub Pages?_

**A:** If you're happy with Jekyll (and it's a fine static site generator for lots of folks), having it integrated on the backend with GitHub Pages is a super-sweet setup. A simple `git push` and the site automatically rebuilds---what more could you want? However, the GitHub Pages version of Jekyll is limited in the plugins you can use, and that in turn limits the functionality of the site. Now, I could've stuck with GitHub Pages as a hosting solution and not used the Jekyll functionality, as it is true that GitHub Pages offers the ability to host static sites without using Jekyll. The workflow for such a setup just didn't feel as straightforward as I would've liked (it involved multiple branches, including the required "gh-pages" branch, and Git submodules), so it didn't seem like there was really any point. From my perspective, GitHub Pages and Jekyll had a shared fate---there wasn't much point in using one without the other.

_**Q:** Why choose Hugo?_

**A:** A lot of this is covered [in my previous post][xref-1], but I'll quickly re-iterate the reasons. First, Hugo is _vastly_ faster at site generation (think 5 minutes for Jekyll down to single-digit seconds for Hugo). Second, Hugo is a single binary that's easily installed on macOS or Linux. Third, and finally, I wanted to stretch my skills, and this seemed like a reasonable way to do it.

_**Q:** Why choose Amazon S3 for hosting your site?_

**A:** As I mentioned above, I wanted to stretch my skills a bit. Hosting the site on [Amazon S3][link-8] gave me the ability to gain additional experience with using and managing services on Amazon Web Services. Amazon S3 is perfectly capable of hosting static sites, so why not?

_**Q:** Why use CloudFront in addition to S3?_

**A:** Using S3 alone is perfectly fine for some lower-traffic sites, but I don't really consider this site to be a lower-traffic site (not trying to make myself sound more important/influential than I am, just being realistic). I felt it would be better to leverage [CloudFront][link-9] as a content distribution network (CDN) and be able to offer (hopefully) lower-latency service to more readers around the world. Plus, this is another way to stretch my skills and my experience.

_**Q:** Is there a reason you're not using a continuous integration (CI) service (like [TravisCI][link-7], [CircleCI][link-6], or [AWS CodePipeline][link-5]) in your setup?_

**A:** This is something that I really wanted to do, and I may yet work this into my workflow. The general way this works is that you connect the CI service to [GitHub][link-4] via a webhook so that a commit to the repository automatically triggers the site generation and subsequently uploads to S3. I avoided adding this step in the first iteration in order to keep the workflow simple and easy to understand. There are a couple of things I need to tackle in order to add a CI service. First, because many of these services are [Docker][link-12]-based, I need to build an appropriate Docker image that contains the tools I need (Hugo, the [AWS CLI][link-11], and [`s3deploy`][link-10]). Second, I need to test the Docker image to ensure that I'm not unecessarily uploading files that I don't need to upload (`s3deploy` is supposed to help with this, but I haven't tested it in an ephemeral container yet). Third, I need to understand the configuration of the overall workflow and pipeline itself. Fourth, I need to do some thinking on what "continuous integration" means for a static site; is this just verifying that the site builds without any errors?

These are all achievable things; it's just a matter of finding the time to work on them. The good news is that all these things are also really good blog post topics.

_**Q:** Are you adding comments back to the site?_

**A:** Hugo doesn't have a built-in commenting system, since it's made for static sites. There are, though, a few ways of adding comments to a static site. I won't do Disqus (for a variety of reasons), but there is a solution that leverages GitHub pull requests for comments. There's also a solution based on [AWS Lambda][link-13] as well. This topic is something I'll need to explore in greater detail, especially if I'm going to use it in conjunction with a CI service.

There you have it---feel free to [hit me up on Twitter][link-14] if you have additional questions.



[link-1]: https://pages.github.com/
[link-2]: https://jekyllrb.com/
[link-3]: https://gohugo.io/
[link-4]: https://www.github.com/
[link-5]: https://aws.amazon.com/codepipeline/
[link-6]: https://circleci.com/
[link-7]: https://travis-ci.com/
[link-8]: https://aws.amazon.com/s3/
[link-9]: https://aws.amazon.com/cloudfront/
[link-10]: https://github.com/bep/s3deploy
[link-11]: https://aws.amazon.com/cli/
[link-12]: https://www.docker.com/
[link-13]: https://aws.amazon.com/lambda/
[link-14]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2017-08-10-information-recent-site-migration.md" >}}
