---
author: slowe
categories: Explanation
comments: true
date: 2024-07-15T08:15:00-06:00
tags:
- CLI
- Git
- macOS
- Linux
title: Using a Git Commit Template
url: /2024/07/15/using-a-git-commit-template/
---

Although I'm sure that I'd seen or read about Git commit templates previously, the idea of using a Git commit template was brought to the forefront recently with the release of version 11 of Tower for Mac. When I'm using macOS, Tower is my graphical Git client of choice. However, most of my Git operations are via the terminal, and I'm not always using macOS (I also spend a fair amount of time using Linux). For that reason, I wanted to implement a more "platform-neutral" solution to Git commit templates. In this post, I'll share with you what I learned about using a Git commit template.<!--more-->

Let me start with explaining why I wanted to start using a Git commit template. Some time ago, I was exposed to [the Conventional Commits specification][link-3], and decided I wanted to adopt this for my personal projects. As with starting any new habit, it can be challenging to remember, and I found myself committing changes to personal repositories without following the specification. So when I was reminded of Git commit template functionality via its inclusion in [Tower][link-1], I immediately realized this would be a great way to help build new habits (and possibly simplify/streamline things along the way!).

There are times when using a graphical [Git][link-2] client like Tower is extraordinarily useful (it can be super useful to quickly see which branches have been pushed to which remotes, for example, or to visualize the relationships between branches). However, given that the majority of the time I'm performing commits from the terminal, I didn't want to rely upon Tower for commit template functionality. Further, while Tower is available for macOS and Windows, it isn't available for Linux. I needed to leverage a Git commit template in a way that would work for both platforms.

Fortunately, this is a well-documented process with numerous examples accessible via a web search, as I quickly discovered by reading a few resources from other authors (such as [here][link-4], [here][link-5], and [here][link-6]). There are two parts: first, creating the actual message template; and second, configuring Git to use the template.

Creating the commit message template involves only using your text editor of choice to add whatever text you want into a file (as you can tell from the links in the previous paragraph, using a "hidden" file like `~/.gitmessage` or `~/.git-commit-template.txt` is pretty common). I opted for a minimal template  that incorporated a few ideas from others:

```text
<type>: <title>
# No more than 50 chars ##### 50 chars is here: #

# Leave a blank line between title and body
<body>
# Wrap at 72 chars ################################ 72 chars is here: #
```

As you can see, I added placeholders to follow the Conventional Commit specification. I also included text guides to help me ensure the commit title and commit body weren't too long (a common problem for me). You could add more, if you wish; again, the links above provide some great examples.

Once the template is created, then it's time to configure Git to use the template:

```bash
git config --global commit.template <path-to-template-file>
```

This sets it globally, but you could set it on a per-repository basis (omit the `--global` flag and run the command from within a Git repository) or even incorporate it into a [conditional git configuration][xref-1]. I'm a fan of the latter approach, as it lets you set up different commit templates for personal projects and work projects (because commit message requirements might be different at work).

The entire process I've described here is the same for both macOS and Linux. Windows will be similar, but the details will vary slightly.

That's all there is to it. Even though this is a straightforward and well-documented process, I've found it enormously helpful. I hope you do, too! Feel free to reach out to me and let me know---I'm available [on Twitter][link-7], on [the Fediverse/Mastodon][link-8], and in various open source Slack communities. I'd love to hear from you.

[link-1]: https://www.git-tower.com/mac
[link-2]: https://git-scm.com/
[link-3]: https://www.conventionalcommits.org/en/v1.0.0/
[link-4]: https://gist.github.com/lisawolderiksen/a7b99d94c92c6671181611be1641c733
[link-5]: https://thoughtbot.com/blog/better-commit-messages-with-a-gitmessage-template
[link-6]: https://gist.github.com/adeekshith/cd4c95a064977cdc6c50
[link-7]: https://twitter.com/scott_lowe
[link-8]: https://fosstodon.org/@scottslowe
[xref-1]: {{< relref "2023-12-15-conditional-git-configuration.md" >}}
