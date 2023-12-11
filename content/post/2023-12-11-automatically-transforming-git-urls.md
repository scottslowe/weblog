---
author: slowe
categories: Education
comments: true
date: 2023-12-11T13:30:00-06:00
tags:
- Git
- CLI
title: Automatically Transforming Git URLs
url: /2023/12/11/automatically-transforming-git-urls
---

[Git][link-1] is one of those tools that lots of people use, but few people truly master. I'm still on my own journey of Git mastery, and still have so very far to go. However, I did take one small step forward recently with the discovery of the ability for Git to automatically rewrite remote URLs. In this post, I'll show you how to configure Git to automatically transform the URLs of Git remotes.<!--more-->

The key here is the `url` configuration stanza and the associated `insteadOf` keyword. Added to your Git configuration---either globally or on a per-repository basis---these configuration options will tell Git to use a different URL every time it encounters the specified original URL.

Here's an example:

```toml
[url "git@github.com:org/"]
    insteadOf = "https://github.com/org/"
```

The `git@github.com:org/` is the _replacement_ URL; that is, the URL that you want Git to use. The URL specified by the `insteadOf` keyword is the _original_ URL; that is, the URL you want Git to replace. As you can see in the example, it's possible not only to transform HTTPS-based URLs to SSH URLs (or vice versa), but it's possible to constrain this transformation to repositories belonging to a specific organization or user. Being able to constrain the transformation of URLs is extraordinarily handy; I use this functionality to use SSH URLs for my work-related repositories automatically without affecting other GitHub-based URLs.

With this configuration in place, I could do this:

```shell
git clone https://github.com/org/repository.git
```

But running `git remote -v` in the newly-cloned repository would show this:

```shell
origin    git@github.com:org/repository.git (fetch)
origin    git@github.com:org/repository.git (push)
```

Useful, wouldn't you say?

Got any other nifty but perhaps not-so-well-known Git features? Hit me up and share your favorite! You can reach me via a few different Slack communities ([Pulumi][link-2], [Kubernetes][link-3]), [on the Fediverse][link-4], and [on Twitter][link-5]. I'd love to hear from you!

[link-1]: https://www.git-scm.com/
[link-2]: https://pulumi-community.slack.com/
[link-3]: https://kubernetes.slack.com/
[link-4]: https://fosstodon.org/@scottslowe
[link-5]: https://twitter.com/scott_lowe
