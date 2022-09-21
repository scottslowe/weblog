---
author: slowe
categories: Explanation
comments: true
date: 2022-09-21T08:30:00-06:00
tags:
- Automation
- IaC
- Pulumi
- YAML
title: Referencing Configuration Values in Pulumi YAML
url: /2022/09/21/referencing-configuration-values-in-pulumi-yaml/
---

Lately I've been doing a fair amount of work with Pulumi's YAML support (see [this blog post][link-1] announcing it), and I recently ran into a situation where I wanted to read in and use a configuration value (set via `pulumi config`). When using one of Pulumi's supported programming languages, like TypeScript or Python or Go, this is pretty easy. It's also easy in YAML, but not as intuitive as I originally expected. In this post, I'll share how to read in and use a configuration value when using Pulumi YAML.<!--more-->

Configuration values are how you parameterize a Pulumi program in order to make it more flexible and reusable (see [this page on configuration][link-3] from Pulumi's architecture and concepts documentation). That same page also has examples of using `config.Get` or `config.Require` to pull configuration values into a program (the difference between these two, by the way, is that the latter will prevent a program from running if the configuration value isn't supplied).

In YAML, it's (currently) handled a bit differently. As outlined in [the Pulumi YAML reference][link-2], a Pulumi YAML document has four main sections: `configuration`, `resources`, `variables`, and `outputs`. At first, I thought I'd have to use the `variables` section along with the `Fn::Invoke` function, which allows users to "call" or "invoke" some sort of programmatic function, like looking up information from a provider or similar. There are other functions as well, like Base64 encoding or decoding.

As it turns out, though, it's actually the `configuration` section that is what you need to read in and use a configuration value. This seems obvious now in retrospect, but it didn't seem obvious at first. In my case, I wanted to read in and use a configuration value for the underlying provider, like the `azure-native:location` configuration value used by the Azure Native provider. Here's what's needed:

```yaml
configuration:
  azure-native:location:
    type: String
```

Omitting a `default: <some value>` line from the stanza above, like I did, means that this is a _required_ value (i.e., it is analogous to using `config.Require` in a programming language).

That's only half the picture, though. To then later reference this value somewhere else, you use the standard Pulumi YAML interpolation syntax. Continuing the example of using the `azure-native:location` configuration value, here's how you would reference that configuration value when creating an Azure resource group:

```yaml
resources:
  resourceGroup:
    type: azure-native:resources:ResourceGroup
    properties:
      location: ${azure-native:location}
      resourceGroupName: scotts-rg
```

While this example shows using a provider configuration value (the `azure-native:location` value), it is equally applicable to _any_ sort of configuration value you need to pass in and reference in a Pulumi YAML document.

I hope this proves useful to someone out there. If you have questions, feel free to reach out to [me on Twitter][link-4], or come find me in [the Pulumi community Slack][link-5].

[link-1]: https://www.pulumi.com/blog/pulumi-yaml/
[link-2]: https://www.pulumi.com/docs/reference/yaml/
[link-3]: https://www.pulumi.com/docs/intro/concepts/config/
[link-4]: https://twitter.com/scott_lowe
[link-5]: https://pulumi-community.slack.com
