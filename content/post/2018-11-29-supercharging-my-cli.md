---
author: slowe
categories: Explanation
comments: true
date: 2018-11-29T12:00:00Z
tags:
- Linux
- Fedora
- CLI
title: Supercharging my CLI
url: /2018/11/29/supercharging-my-cli/
---

I spent a _lot_ of time in the terminal. I can't really explain why; for many things it just feels faster and more comfortable to do them via the command line interface (CLI) instead of via a graphical point-and-click interface. (I'm not totally against GUIs, for some tasks they're far easier.) As a result, when I find tools that make my CLI experience faster/easier/more powerful, that's a big boon. Over the last few months, I've added some tools to my [Fedora][link-7] laptop that have really added some power and flexibility to my CLI environment. In this post, I want to share some details on these tools and how I'm using them.<!--more-->

The tools I've adopted and that I'll discuss in this post are:

* `powerline-go` for an informative CLI prompt
* `rg` for faster content searches
* `fd` for faster filename searches
* `fzf` for fuzzy command history access and faster directory navigation

Let's take a closer look at each of these.

## A More Informative Shell Prompt

There's been quite a few articles written about [`powerline`][link-9], a Python-based utility that provides a much more informative shell prompt. Instead of going down the traditional `powerline` route, I found [`powerline-go`][link-8]---a small, statically linked Go binary that provides similar functionality.

Now, my shell prompt automatically displays things like:

* The currently checked-out branch in a Git repository and indicators for untracked/modified files in the repository
* The name of the currently-active Python virtual environment
* The name of the Kubernetes cluster against which `kubectl` will operate (and the namespace, if one is selected)
* And more mundane information like the username, hostname, and current working directory

Here's a quick demo of `powerline-go`:

![Powerline-go demo](/public/img/powerline-go-demo.gif)

I've found `powerline-go` to be very easy to use and very easy to configure. If you're looking for a way to make your shell prompt a bit more interactive and a bit more informative, `powerline-go` is definitely worth a look.

## Faster Content Searches with Ripgrep

[Ripgrep][link-4], or `rg`, is an insanely fast search tool that recursively searches a path for a regex (regular expression) pattern. Ripgrep is faster than most other search tools, ignores binary files by default, and honors `.gitignore` files (which can be both handy and annoying at times). Ripgrep also offers pretty impressive platform support, with releases available for virtually all major Linux distributions, various BSDs, Windows, and macOS.

This is probably subjective, but I find myself now more likely to use `rg` to find a file that I need than I would have before with some other search tool. When I combine it with other tools, it's really become quite handy. That being said, the "regular" `grep` utility may be sufficient for some users' needs.

## Faster Filename Searches with `fd`

[`fd`][link-5] is a super-fast alternative to the traditional `find` command. It offers a simpler syntax (just `fd <pattern>` and you're off to the races) and honors `.gitignore` files by default (although there is an option to not honor them and include hidden/ignored items). You can use it to find only files or only directories in a particular path. (Did I mention it's really fast?)

Like with `rg`, the ease of use and speed of `fd` have led me to use it more often than I would have used the traditional `find` utility. Making it more approachable to perform filename searches has, for me, made the tool more useful in the end.

## Fuzzy Searching with `fzf`

This tool has changed my life.

OK, that's probably hyperbole, but [`fzf`][link-6] is a truly amazing and useful tool. `fzf` describes itself as a "command-line fuzzy finder," and it can be used to perform fuzzy searching in any list of things---filenames, directories, processes, URLs, whatever. Like `rg` and `fd`, it's super-fast.

Out of the box (so to speak), `fzf` enhances the CLI in a couple different ways:

* It replaces the default Ctrl-R history search with a fuzzy history search that lets me search and interactively select commands to run from the command history. (What's that? You didn't know about the Ctrl-R command history search in Bash? Your CLI experience is definitely about to change!)
* `fzf` adds a new keybinding for Ctrl-T, which lets me interactively search for and add parameters to a command-line argument. For example, let's say I want to edit a file in [Visual Studio Code][link-10] (my current preferred text editor). I can type `code`, press Ctrl-T, then fuzzy search for the file or directory I want to open, press Enter to select it, and press Enter again to execute the command. My `fzf` configuration for the Ctrl-T functionality uses `fd` to search anywhere in my home directory, but that's configurable.
* Finally, `fzf` adds a keybinding for Alt-C, which adds the ability to use `fzf` to quickly change directories. Again, `fzf` is leveraging `fd` with a list of all the directories in my home directory, but the tool that `fzf` uses and the scope of the search are configurable.

These three features alone make `fzf` so useful. But wait---there's more! `fzf` can also perform Bash completion for files and directories, process IDs, host names, and environment variables.

It's also highly customizable; see [the GitHub repo][link-6] for more details.

## Combining These Tools

I mentioned above that `fzf` can use `fd` for various functions (like the Alt-C and Ctrl-T functions), so you can already get an idea of how useful it can be to combine these tools with each other or with other tools.

Here's a couple examples:

* Let's say I know I have a file containing the word "Kubernetes" in my home directory, and I remember part of the filename but not the full filename. No worries, `rg` and `fzf` to the rescue! From my home directory, I can run `rg -il kubernetes . | fzf | xargs code`. `rg` will find all the matching files, pass them to `fzf` for interactive fuzzy searching, and then pass the one I select to `code` for editing.
* The `fzf` wiki has an example of using `fzf` for fuzzy searching of Chrome browsing history; I adapted this for Firefox. (See the `fwh` file in [my "scripts" repository][link-11]. I'm still fine-tuning it for the best results.) There's also a _ton_ of other examples in the wiki, so have a look there.

I'm also working on a command/script that will use both `rg` _and_ `fd` for a combined filename/content search that passes the unique results back to `fzf` for selecting a file to edit. I haven't quite gotten that one worked out just yet.

## Other Tools

Other tools I'm currently testing (but haven't necessarily fully adopted as core part of my workflow) are [pet][link-1] (described as a command-line snippet manager) and [memo][link-2] (a tool for creating/editing/finding "memos" or short notes). Both of these tools can also leverage `fzf` for finding information, so that brings a bit of consistency to my toolset (fuzzy searching for the win!).

Anyway, I hope you found something useful here, and feel free to [hit me up on Twitter][link-3] if you have questions or suggestions for other tools I should consider. Thanks!

[link-1]: https://github.com/knqyf263/pet
[link-2]: https://github.com/mattn/memo
[link-3]: https://twitter.com/scott_lowe
[link-4]: https://github.com/BurntSushi/ripgrep
[link-5]: https://github.com/sharkdp/fd
[link-6]: https://github.com/junegunn/fzf
[link-7]: https://getfedora.org
[link-8]: https://github.com/justjanne/powerline-go
[link-9]: https://powerline.readthedocs.io/en/latest/
[link-10]: https://code.visualstudio.com/
[link-11]: https://github.com/scottslowe/scripts
