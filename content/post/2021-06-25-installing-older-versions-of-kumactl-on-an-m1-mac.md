---
author: slowe
categories: Tutorial
comments: true
date: 2021-06-25T15:30:00-06:00
tags:
- CLI
- macOS
- Kong
- Kuma
title: Installing Older Versions of Kumactl on an M1 Mac
url: /2021/06/25/installing-older-versions-of-kumactl-on-an-m1-mac/
---

The Kuma community [recently released version 1.2.0][link-1] of the open source Kuma service mesh, and along with it a corresponding version of `kumactl`, the command-line utility for interacting with Kuma. To make it easy for macOS users to get `kumactl`, the Kuma community maintains a [Homebrew][link-2] formula for the CLI utility. That includes providing M1-native (ARM64) macOS binaries for `kumactl`. Unfortunately, installing an earlier version of `kumactl` on an M1-based Mac using Homebrew is somewhat less than ideal. Here's one way---probably not the only way---to work around some of the challenges.<!--more-->

Note that this post really only applies to users of M1-based Macs; users of Intel-based Macs can extract the `kumactl` binary from the release archive available linked from [the Kuma install docs][link-4]. (The same goes for users of Linux distributions running on Intel-based hardware.) On the Kuma website, simply select the desired version of Kuma from the drop-down in the upper left, check the page for the direct download link, and off you go. This doesn't work for M1-based Macs because at the time this post was written, the Kuma community was not providing ARM64 binaries. This leaves Homebrew as the only way (aside from building it yourself from source) to get a native ARM64 binary of `kumactl` for an M1-based Mac.

There are a couple of things to point out---caveats, if you will---about this workaround before I get into the details.

1. It seems (based what I've observed regarding Homebrew's behavior) that the ability to have multiple versions installed via Homebrew relies on what I'm calling "versioned formulas," like `terraform@0.13` (where the version number is embedded in the name of the formula).
2. Although you can have multiple versions of a formula present on the disk in the correct locations, that doesn't necessarily mean that Homebrew will recognize them as "installed."
3. Without Homebrew recognizing the packages as "installed," then using commands like `brew unlink` or `brew link` will be severely limited. This is particularly impactful in this situation.

With these limitations in mind, let's look at the workaround I've found. It's based on information from a variety of sources; [this one][link-5] and [this one][link-6] are good examples. It was [this GitHub gist][link-3], however, that finally got me to success. The approaches that describe using a remote URL failed with a Homebrew error for me (this is using Homebrew 3.2.0).

Here's the steps that worked for me, based very largely on the referenced GitHub gist:

1. Navigate to the directory where the Homebrew formulas are stored. On my M1-based system, that location was `/opt/homebrew/Library/Taps/homebrew/homebrew-core/Formula`. You can use `brew --prefix` to determine the base location where Homebrew is installed; the rest of the path should be the same.
2. As it turns out, the directory where Homebrew is installed is a giant Git repository. Thus, we can "roll back" to the state of the repository when the previous version of the formula you want is available. To figure that out for `kumactl`, use `git log --follow kumactl.rb`. Examine the output to determine the correct Git commit that will get you the desired version. I needed version 1.1.6 of `kumactl`, so the Git commit I needed was `89b6866`.
3. Run `git checkout -b kumactl-1.1.6 89b6866` to create a new branch at that commit hash.
4. Reinstall the older version with `brew reinstall ./kumactl.rb`.
5. If you have a newer version installed, this command _will remove the newer version._

If you only needed to roll back to a previous `kumactl` version, then you're good to go. If you need multiple versions, read on. The following steps will help you get multiple copies installed (with some limitations):

1. Copy the `/opt/homebrew/Cellar/kumactl/<version>` directory to a backup location on disk somewhere. You can replace `/opt/homebrew` with `$(brew --prefix)` if you're not sure where Homebrew is installed.
2. Assuming you're still inside the `/opt/homebrew/Library/Taps/homebrew/homebrew-core/Formula` directory, run `git checkout master` to switch out of the branch you created earlier in order to install the older version.
3. Run `brew install kumactl` to install the latest version. _This will remove the earlier version._
4. Copy the `<version>` directory from step 1 from the backup location to `/opt/homebrew/Cellar/kumactl`. In my example, this meant I had both a `1.1.6` and a `1.2.0` directory in `/opt/homebrew/Cellar/kumactl`.

You now have multiple versions installed. **However**, due to the caveats/limitations listed earlier, Homebrew doesn't recognize both versions as "installed." This is in spite of the fact that `brew info kumactl` shows that it recognizes both versions are present on the disk. Since both versions aren't recognized as "installed," this means you _cannot_ use `brew unlink` or `brew link` to switch between versions. (Some of the articles I found also referenced a `brew switch` command, which apparently does the same function but appears to have been removed from the latest versions of Homebrew.)

So what to do? Probably the easiest thing to do is this:

    ln -s $HOME/bin/kumactl-1.1.6 /opt/homebrew/Cellar/kumactl/1.1.6/bin/kumactl

Replace `$HOME/bin` with the path of your choice, preferably something already in `$PATH`. This allows you to run the older version by specifying the binary (via symbolic link) with the version in the name, and running the latest version by just using the binary's name. It's not a great solution, but it works.

I hope this (somewhat limited) workaround is useful. There are discussions happening in the Kuma community about what, if anything, the community can do to help solve this problem. Join [the Kuma community Slack][link-7] and have your opinions heard---or, even better, offer to help! In the meantime, feel free to hit [me on Twitter][link-8] if you have any questions, or if you have a better solution to this problem.

[link-1]: https://kuma.io/blog/2021/kuma-1-2-0/
[link-2]: https://brew.sh
[link-3]: https://gist.github.com/sawant/6ea10e45de7d09e966a1dc6afea3bffb
[link-4]: https://kuma.io/docs/1.2.0/installation/macos/
[link-5]: https://flaviocopes.com/homebrew-install-older-version/
[link-6]: https://itnext.io/how-to-install-an-older-brew-package-add141e58d32
[link-7]: https://chat.kuma.io
[link-8]: https://twitter.com/scott_lowe
