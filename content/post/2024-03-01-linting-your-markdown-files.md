---
author: slowe
categories: Explanation
comments: true
date: 2024-03-01T10:00:00-06:00
tags:
- Markdown
- CLI
title: Linting your Markdown Files
url: /2024/03/01/linting-your-markdown-files/
---

It's no secret I'm a fan of Markdown. The earliest mention of Markdown on this site is all the way back in 2011, and it was only a couple years after that when I migrated this site from WordPress to Markdown. Back then, the site was generated from Markdown using Jekyll (via GitHub Pages); today it is generated from Markdown sources using Hugo. One thing I've not done, though, is perform linting (checking for errors or potential errors) of the Markdown source files. That's all about to change! In this post, I'll share with you how I started linting my Markdown files.<!--more-->

To handle the linting, there are (at least) a couple different options:

1. markdownlint-cli ([GitHub repository][link-1])
2. markdownlint-cli2 ([GitHub repository][link-2])

Both of these use the same [`markdownlint` library][link-3] under the hood. They're both available as both a CLI tool or as a [Docker][link-5] container; `markdownlint-cli2` is also available as a [GitHub Action][link-4]. In both cases, the CLI tool is installed via `npm install` (typically globally with `--global` or `-g`). The key difference between the two is that `markdownlint-cli2` is configuration-driven, whereas `markdownlint-cli` offers the ability to use either a configuration file or command-line flags. I decided to use `markdownlint-cli`, as the ability to use command-line flags makes it a tad easier to get started.

I performed initial testing with the Docker container, which you would tend to invoke like this:

```bash
docker container run --rm -v "$PWD:/workdir" ghcr.io/igorshubovych/markdownlint-cli:latest "path/to/*.md"
```

However, I later switched to the CLI tool for better cross-platform portability (yes, I know that macOS can run Docker containers via Docker Desktop, but you still have to pay the tax of running a Linux VM in the background). The CLI tool is invoked in much the same way:

```bash
markdownlint "path/to/*.md"
```

In the default configuration, `markdownlint-cli` flagged a _lot_ of violations in the over 2,200 blog posts on the site. After fine-tuning the configuration by disabling a few rules (more details on the rules is found [here][link-7]), there were still a lot of violations---but not nearly as many. Notably, I disabled MD013 ("line-length") and MD052 ("reference-links-images"); the former because I use soft line-wraps in my Markdown paragraphs and the latter because I use Hugo's `relref` shortcode for cross-referencing other posts.

Initially it was a bit unclear to me how to use the `.markdownlint.jsonc` configuration file to disable some of the rules. (This was probably just me being dense, if I'm honest.) For example, a configuration for MD052 might look like this:

```json
// Rule details : https://github.com/DavidAnson/markdownlint/blob/v0.33.0/doc/md052.md
"reference-links-images": {
    "shortcut_syntax": false
},
```

To disable this rule, it needs to look like this:

```json
// Rule details : https://github.com/DavidAnson/markdownlint/blob/v0.33.0/doc/md052.md
"reference-links-images": false,
```

In retrospect, setting the top-level entry to `false` is obvious now, but when I first started looking at the configuration file I was expecting a property like `disabled: true` or similar.

Even with a few rules disabled, there were still quite a few violations, which I fixed manually over the course of a couple weeks, until I was finally able to run `markdownlint` over the entire list of ~2,230 Markdown posts without any violations. Yay!

The next step was to automate the process of running the Markdown lint checks---but that's a topic for a separate post!

## Additional Resources

While researching what was involved in linting Markdown files, I found [this post][link-6] to be helpful in getting started with `markdownlint`. The GitHub repositories ([here][link-1], [here][link-2], and [here][link-3]) were, of course, also very helpful (especially the [rule descriptions][link-7]).

I hope this post is useful to some folks out there. Please feel free to reach out to [me on Twitter][link-8] or [on the Fediverse][link-9] if you have comments, questions, or feedback (on this post or any post on my site). Thanks for reading!

[link-1]: https://github.com/igorshubovych/markdownlint-cli
[link-2]: https://github.com/DavidAnson/markdownlint-cli2
[link-3]: https://github.com/DavidAnson/markdownlint
[link-4]: https://github.com/actions
[link-5]: https://www.docker.com/
[link-6]: https://emmer.dev/blog/linting-markdown-files-with-markdownlint/
[link-7]: https://github.com/DavidAnson/markdownlint/blob/main/doc/Rules.md
[link-8]: https://twitter.com/scott_lowe
[link-9]: https://fosstodon.org/@scottslowe
