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

Normally, installing a [Pulumi][link-2] provider is pretty easy; you run `pulumi up` and the provider gets installed automatically. Worst case scenario, you can install the provider using `pulumi plugin install`. However, sometimes things have to be done manually. In this post, I'll walk you through installing a prerelease provider---in this case, the prerelease Pulumi provider for [Talos Linux][link-1]---using both `pulumi plugin install` as well as using a manual process.<!--more-->

Currently, the prerelease provider for Talos Linux (found in [this GitHub repository][link-3]) can't be installed automatically when running `pulumi up`. So, to use the Talos Linux provider, you'll need to install it before running `pulumi up`.

To install it using `pulumi plugin install`, you need to know the version you want to install. As of this writing, the latest release was v0.1.0-beta.0. Armed with that information, you can install the plugin with this command:

```shell
pulumi plugin install resource talos v0.1.0-beta.0 --server github://api.github.com/siderolabs/pulumi-provider-talos
```

If the GitHub repository had been named "pulumi-talos" instead of "pulumi-provider-talos", the previous command could be shortened:

```shell
pulumi plugin install resource talos v0.1.0-beta.0 --server github://api.github.com/siderolabs
```

This is because the `pulumi plugin install` command will automatically look for a repository named `pulumi-<provider>`.

If, for whatever reason, you wish to install the plugin manually, the process looks like this:

1. Download the latest release of the Talos provider from [the GitHub Releases page][link-4]. This will download a tarred and gzipped archive.
2. The plugin files need to go into a specific subdirectory under `~/.pulumi/plugins`. Navigate to that directory, and create a subdirectory whose name corresponds to the version of the Talos provider. For example, if the version downloaded is v0.1.0-beta.0, then the name of the new subdirectory should be `resource-talos-v0.1.0-beta.0`.
3. Extract the contents of the release downloaded in step 1 to the new subdirectory created in step 2. You can use `tar -C ~/.pulumi/plugins/<new-subdirectory> -xzf <release-archive>` to extract directly into the new subdirectory created in step 2.
4. Run `pulumi plugin ls`. You should now see the Talos provider listed.

Once you see the provider listed with `pulumi plugin ls`, you're ready to run `pulumi up` with programs that leverage the Talos provider.

If you have any questions about installing the plugin, please feel free to reach out to me; I'll do my best to help. You can contact me [on Twitter][link-5], [on Mastodon][link-6], or via one of the various Slack communities I frequent. Enjoy using your new Talos provider with Pulumi!

**UPDATE 2023-02-10:** I've updated the post with some additional information provided via Ringo De Smet, a colleague of mine at Pulumi. Thank you, Ringo!

[link-1]: https://talos.dev
[link-2]: https://www.pulumi.com
[link-3]: https://github.com/siderolabs/pulumi-provider-talos
[link-4]: https://github.com/siderolabs/pulumi-provider-talos/releases
[link-5]: https://twitter.com/scott_lowe
[link-6]: https://fosstodon.org/@scottslowe
