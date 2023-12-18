---
author: slowe
categories: Explanation
comments: true
date: 2023-12-18T08:30:00-06:00
tags:
- Azure
- CLI
- Linux
- macOS
title: Dynamically Enabling the Azure CLI with Direnv
url: /2023/12/18/dynamically-enabling-azure-cli-with-direnv/
---

I'm a big fan of [`direnv`][link-1], the tool that lets you load and unload environment variables depending on the current directory. It's so very useful! Not too terribly long ago, I wanted to find a way to "dynamically activate" the [Azure CLI][link-2] using `direnv`. Basically, I wanted to be able to have the Azure CLI disabled (no configuration information) unless I was in a directory where I needed or wanted it to be active, and be able to make it active using `direnv`. I finally found a way to make it work, and in this blog post I'll share how you can do this, too.<!--more-->

First, you'll need both `direnv` and the Azure CLI installed (obviously). I'll leave this as an exercise for the readers, but I'll mention that if you want to use Azure CLI in a Python virtual environment you might find [this article][link-3] really helpful.

Next, you'll want to create a couple of directories. I chose to "hide" these directories in a `.config` directory in my home directory. This directory is very commonly found (and used) on many Linux systems, but doesn't typically exist on a macOS system. You can use this command to create the necessary directories:

```shell
mkdir -p ~/.config/azure{,-fake}
```

The `-p` tells `mkdir` to create the directory tree structure as needed, and the use of curly braces here means you'll end up with both an `azure` directory and an `azure-fake` directory. One of these will be the "real" directory where a valid Azure CLI configuration is stored; the other is a "fake" directory that will remain empty. The names aren't terribly important, but keep track of what names you use as you'll need them shortly.

Third, you'll configure your shell startup file(s) to point to the fake directory (`azure-fake`, in my case). This is generally the same for both Bash and Zsh, and involves adding a line to your `~/.bashrc` or `~/.zshrc`:

```shell
export AZURE_CONFIG_DIR="$HOME/.config/azure-fake"
```

With this environment variable in place, the Azure CLI will be inactive, unable to function because it has no configuration.

The next step depends on whether you've ever configured or used the Azure CLI previously, or if this is a completely fresh installation:

* If the _former_ (you've configured or used the Azure CLI previously), then copy everything from the current Azure configuration directory---which defaults to `~/.azure` on macOS and Linux, if I'm not mistaken---to the new directory you created earlier. **Don't copy it to the fake directory specified in the environment variable above!** Once you've copied it, remove the old `~/.azure` directory.
* If the _latter_ (this is a fresh installation), no additional steps are needed (yet).

The final step is to activate the Azure CLI where and when needed. This is done by setting the `AZURE_CONFIG_DIR` environment variable to point to the "real" directory you created earlier (in my case, it was `~/.config/azure`). Since we're talking about using `direnv`, then put this into an `.envrc` file in the directory where you want or need the Azure CLI to be active:

```shell
export AZURE_CONFIG_DIR="$HOME/.config/azure"
```

If you'd already configured or used the Azure CLI, then when you are in a directory where `.envrc` contains this line the Azure CLI will pick up the configuration you copied over earlier and should just work (depending on your credentials and such you might need to do an `az login`). If this is a completely fresh installation of the Azure CLI, then you'll definitely need to do an `az login`, and then you should be good to go.

When you change out of a directory where `.envrc` has been configured to activate the Azure CLI, then the default value for `AZURE_CONFIG_DIR` kicks in---taken from your shell's startup file---and the Azure CLI will be inactive again because it has no configuration. (It's important to remember **NOT** to run `az login` when the fake directory is active.)

And that's it! The essence of this trick is pointing the `AZURE_CONFIG_DIR` to an empty directory with a non-existent Azure CLI configuration by default, then selectively pointing to a valid Azure CLI configuration using `direnv` and an `.envrc` file.

I hope you find this useful. If you have any feedback for me, I'm active [on the Fediverse][link-4] and [on Twitter][link-5], and I frequent a number of different Slack communities. I'd love to hear from you!

[link-1]: https://direnv.net/
[link-2]: https://learn.microsoft.com/en-us/cli/azure/
[link-3]: https://erick.navarro.io/blog/activate-python-virtualenv-automatically-with-direnv/
[link-4]: https://fosstodon.org/@scottslowe
[link-5]: https://twitter.com/scott_lowe
