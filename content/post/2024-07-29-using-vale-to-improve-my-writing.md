---
author: slowe
categories: Tutorial
comments: true
date: 2024-07-29T06:00:00-04:00
tags:
- Blogging
- CLI
- Writing
- macOS
- Linux
title: Using Vale to Improve my Writing
url: /2024/07/29/using-vale-to-improve-my-writing/
---

Back in March of this year, I talked about how I started using `markdownlint-cli` to perform linting against the Markdown source files that are used by Hugo to generate this site. At the same time, I also started exploring the use of similar tools to check (or lint, if you will) my writing itself. In this post, I'll share with you how I started using Vale to perform some checks against my writing.<!--more-->

More details on my use of `markdownlint-cli` are available [here][xref-1] for reference. `markdownlint-cli` checks for the _structure_ and _formatting_ of Markdown files, but it doesn't do any "higher level" checks regarding the writing itself. For that, I needed to add a second tool, and I opted to use [Vale][link-1], an open source tool specifically aimed at "linting your prose." Among other things, what I liked about Vale was that it offers integration with graphical editors like Visual Studio Code (what I use when I'm on macOS) and Sublime Text (what I use when I'm on Linux), but it also can be run directly from the command-line. And, if you are so inclined, there's [a GitHub Action for Vale][link-2], too. Nice!

The first step is to install Vale:

* On macOS, you can use `brew` to install Vale; just run `brew install vale`.
* On Linux, Vale is available via most distributions' package manager. On Arch Linux, just use `pacman` to install the `vale` package.
* On Windows---well, I don't know. I don't use Windows (the docs say you can use Chocolatey).

After you get Vale installed, then you need to install some "styles" for Vale to use in checking your prose. These styles provide the rules and the checks that Vale performs against your writing, so your choice of styles will affect the way in which Vale evaluates your writing. I chose to use three relatively basic styles:

* [Proselint][link-3]
* [Alex][link-4]
* [Readability][link-5]

Installing these styles isn't hard, but you do need to know where to put the files. To install, download the latest release, unzip it, and then put the unzipped directory with the style's name into something called your "StylesPath." Where is your "StylesPath"? That will vary from OS to OS, but this command will show you the directories that Vale is using:

```bash
vale ls-dirs
```

For example, on macOS the "StylesPath" as reported by `vale ls-dirs` is shown to be `${HOME}/Library/Application Support/vale/styles`. This is where you'll want to place any styles you want Vale to use.

[This GitHub repository][link-6] is an great place to find a number of styles you can download and use.

The last step is to provide a Vale configuration file. There is a system-wide configuration file (the name and location is reported by `vale ls-dirs`), but you can also use per-directory configuration files (similar to how EditorConfig works, if you're familiar with that).

Vale configuration files can be quite short. As an example, here's the Vale configuration file I use:

```ini
[*.md]
BasedOnStyles = Readability,alex,proselint
```

This two-line configuration file tells Vale to use the Readability, alex, and proselint styles when linting the prose in my blog posts.

Once Vale has styles and a configuration file, you can invoke it by running `vale <name-of-markdown-file>`. Here's the output of running Vale against this blog post:

```text
✔ 0 errors, 0 warnings and 0 suggestions in 1 file.
```

I usually struggle with the readability scores, but this post seems to be an exception. Maybe that means Vale is indeed helping to improve my writing?

I hope this is useful. I love to hear from readers, so feel free to contact me via e-mail (my address is on this site and isn't hard to find), [via Twitter][link-7], [via Mastodon][link-8], or via one of the various Slack communities that I frequent. Thanks for reading!

[link-1]: https://vale.sh/
[link-2]: https://github.com/errata-ai/vale-action
[link-3]: https://github.com/errata-ai/proselint
[link-4]: https://github.com/errata-ai/alex
[link-5]: https://github.com/errata-ai/readability
[link-6]: https://github.com/errata-ai/packages
[link-7]: https://twitter.com/scott_lowe
[link-8]: https://fosstodon.org/@scottslowe
[xref-1]: {{< relref "2024-03-01-linting-your-markdown-files.md" >}}
