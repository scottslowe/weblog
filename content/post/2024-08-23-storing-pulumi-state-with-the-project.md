---
author: slowe
categories: Explanation
comments: true
date: 2024-08-23T08:00:00-06:00
tags:
- CLI
- Pulumi
title: Storing Pulumi State in the Project Directory
url: /2024/08/23/storing-pulumi-state-in-the-project-directory/
---

Pulumi, like Terraform and OpenTofu, has the ability to store its _state_ in a supported _backend_. You can store the state in one of the blob/object storage services offered by the major cloud providers, via Pulumi's SaaS offering (called Pulumi Cloud), or even locally. It's this last option I'll explore a little bit in this post, where I'll show you how to configure Pulumi to store the state in the project directory instead of somewhere else.<!--more-->

Let me start with this disclaimer: If you're working with a team of folks on IaC for your project or employer, **don't do this.** Storing project state locally with your project will just make life difficult for you. Instead, just accept that you need to store the state somewhere that your whole team can access it. Howver, if you are a "team of one" then you might find this interesting or useful.

[Pulumi][link-1] supports a "local" backend, which means storing stack state information locally on the same system where Pulumi is running. By default, Pulumi will store the state information in the `${HOME}/.pulumi` folder.

It's possible to configure the location the local backend uses with the `PULUMI_BACKEND_URL` environment variable (see [this page][link-2] for a full list of environment variables that Pulumi uses). Note that this variable doesn't just affect the local backend; it allows you to configure _any_ supported Pulumi backend. You could specify the name of an S3 bucket Pulumi should use, for example. You could set that value on a per-directory basis using something like [Direnv][link-3] so that different projects (i.e., different directories) could use different backends.

To use the `PULUMI_BACKEND_URL` to configure the local backend, you'll need to use the `file://` URL syntax (as opposed to using `s3://` for an S3 bucket, or an HTTPS URL when using the Pulumi SaaS backend). To configure the local backend to store the state in the current project directory, set `PULUMI_BACKEND_URL` like this:

```bash
export PULUMI_BACKEND_URL=file://.
```

Pulumi will create a `.pulumi` subdirectory in the current directory and store project state there. In this regard, it will make Pulumi more like [Terraform][link-7] and [OpenTofu][link-8], which also (unless configured otherwise) will store state in the current project directory.

Do I recommend doing this? No, not really. You might think, "Well, I'm reducing my dependency on Pulumi's SaaS offering or other cloud storage options", but the reality is that Pulumi orchestrates cloud infrastructure. If you can't get to the cloud to access project state, then you probably can't get to the cloud to orchestrate infrastructure, either.

That said, you may have valid reasons for wanting to store Pulumi state locally, and I tend to prefer this approach instead of the default configuration.

As a side note, using this approach does not remove the need for the `${HOME}/.pulumi` directory, which is still used for Pulumi providers and other data. This approach only changes the location where Pulumi stores project state. If you don't want to use `${HOME}/.pulumi` as the location where Pulumi stores non-project state data, then you can use the `PULUMI_HOME` environment variable to specify a different location.

I hope this is helpful to someone out there! Thanks for reading. If you have questions, comments (maybe I overlooked something?), corrections, or suggestions for improvement, please feel free to reach out. I'm available [on Twitter][link-4], on [the Fediverse][link-5], via e-mail (have a look around on the site), or in [the Pulumi Slack community][link-6].

[link-1]: https://www.pulumi.com/
[link-2]: https://www.pulumi.com/docs/cli/environment-variables/
[link-3]: https://direnv.net/
[link-4]: https://twitter.com/scott_lowe
[link-5]: https://fosstodon.org/@scottslowe
[link-6]: https://slack.pulumi.com
[link-7]: https://www.terraform.io/
[link-8]: https://opentofu.org/
