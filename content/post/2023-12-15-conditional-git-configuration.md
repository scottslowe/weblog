---
author: slowe
categories: Explanation
comments: true
date: 2023-12-15T14:00:00-06:00
tags:
- Git
- CLI
title: Conditional Git Configuration
url: /2023/12/15/conditional-git-configuration/
---

Building on the earlier article on [automatically transforming Git URLs][xref-1], I'm back with another article on a (potentially powerful) feature of [Git][link-1]---the ability to conditionally include Git configuration files. This means you can configure Git to be configured (and behave) differently based on certain conditions, simply by including or not including Git configuration files. Let's look at a pretty straightforward example taken from my own workflow.<!--more-->

Here's a configuration stanza from my own system-wide Git configuration:

```toml
[includeIf "gitdir:~/Work/Code/Repos/"]
    path = ~/Work/Code/Repos/.gitconfig
```

The key here is the `includeIf` keyword. In this case, Git will include the referenced configuration file specified by `path`, _if_ the location of the Git repository matches the path specification after `gitdir`. Basically, what this means is that _all_ repositories under `~/Work/Code/Repos` will trigger the inclusion of the additional configuration file.

Here's the additional configuration file:

```toml
[user]
    email = name@work-domain.com
    name = Scott Lowe
[commit]
    gpgsign = false
```

As long as I group all work-relatd repositories in the specified directory path, these values override the system-wide values. This means I can specify my work e-mail address as the e-mail address to be associated with commits to work-related repositories while all others use a different e-mail address. This configuration also allows me to disable GPG signing of commits for work-related repositories (i.e., repositories in the specified path), since I don't have a GPG key associated with my work e-mail address.

Could you do this with per-repository configuration settings? _Absolutely._ This configuration mechanism allows you to apply configuration settings to groups of repositories based on their filesystem location, instead of having to do the same thing on a per-repository basis.

I hope you find this information useful. Do feel free to hit me up---[on the Fediverse][link-2], [on Twitter][link-3], or in any of a variety of Slack communities---if you have any questions or any feedback!

[link-1]: https://www.git-scm.com
[link-2]: https://fosstodon.org/@scottslowe
[link-3]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2023-12-11-automatically-transforming-git-urls.md" >}}
