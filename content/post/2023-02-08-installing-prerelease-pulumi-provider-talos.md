---
author: slowe
categories: Tutorial
comments: true
date: 2023-02-08T22:00:00-05:00
tags:
- CLI
- Pulumi
- Talos
title: Installing the Prerelease Pulumi Provider for Talos
url: /2023/02/08/installing-prerelease-pulumi-provider-talos/
---

Normally, installing a [Pulumi][link-2] provider is pretty easy; you run `pulumi up` and the provider gets installed automatically. Worst case scenario, you can install the provider using `pulumi plugin install`. However, when dealing with prerelease providers, sometimes things have to be done manually. Such is the case with the prerelease Pulumi provider for [Talos Linux][link-1]. In this post, I'll show you what the manual process looks like for installing a prerelease provider.<!--more-->

The GitHub repository for the prerelease Pulumi provider for Talos can be found [here][link-3]. As of this writing, the latest release was v0.1.0-beta.0. Currently, the prerelease provider for Talos Linux can't be installed automatically when running `pulumi up`, and `pulumi plugin install` doesn't work either.

The manual process for installing this provider looks like this:

1. Download the latest release of the Talos provider from [the GitHub Releases page][link-4]. This will download a tarred and gzipped archive.
2. The plugin files need to go into a specific subdirectory under `~/.pulumi/plugins`. Navigate to that directory, and create a subdirectory whose name corresponds to the version of the Talos provider. For example, if the version downloaded is v0.1.0-beta.0, then the name of the new subdirectory should be `resource-talos-v0.1.0-beta.0`.
3. Extract the contents of the release downloaded in step 1 to the new subdirectory created in step 2. You can use `tar -C ~/.pulumi/plugins/<new-subdirectory> -xzf <release-archive>` to extract directly into the new subdirectory created in step 2.
4. Run `pulumi plugin ls`. You should now see the Talos provider listed.

Once you see the provider listed with `pulumi plugin ls`, you're ready to run `pulumi up` with programs that leverage the Talos provider.

If you have any questions about installing the plugin, please feel free to reach out to me; I'll do my best to help. You can contact me [on Twitter][link-5], [on Mastodon][link-6], or via one of the various Slack communities I frequent. Enjoy using your new Talos provider with Pulumi!

[link-1]: https://talos.dev
[link-2]: https://www.pulumi.com
[link-3]: https://github.com/siderolabs/pulumi-provider-talos
[link-4]: https://github.com/siderolabs/pulumi-provider-talos/releases
[link-5]: https://twitter.com/scott_lowe
[link-6]: https://fosstodon.org/@scottslowe
