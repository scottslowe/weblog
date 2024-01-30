---
author: simenolsen
categories: Information
comments: true
date: 2023-10-12T10:00:00-06:00
tags:
- Pulumi
- ESC
- Security
- guestpost
title: "Guest Post: Moving Secrets Where They Belong"
url: /2023/10/12/guest-post-moving-secrets-where-they-belong/
---

(This is a guest post by Simen A.W. Olsen.)

Pulumi recently shipped [Pulumi ESC][link-1], which adds the "Environment" tab to Pulumi Cloud. For us at Bjerk, this means we can move secrets into a secrets manager like Google Secrets Manager. Let me show you how we did it!<!--more-->

We are already rotating secrets with our [own CLI tool][link-2], which works _fine_, meaning we are getting notifications in our Slack channel---which everyone tends to ignore until something _real_ breaks. If you are curious how we are handling it today, we are using our own [NPM package][link-3] that throws an exception if a secret has expired. To ensure everything works smoothly, we utilize a GitHub Actions workflow that is scheduled to run daily for drift checking.

The secrets are shared between stacks using StackReferences, which has served us well.

## Improving security

One issue with our current setup is that we publicly store encrypted secrets in our repository. Previously, we've thought of using Google Secrets Manager with the [`GetSecret`][link-4] function. That comes with its own territory, such as permissions to the secret and managing those permissions---not to mention that we already use multiple secret managers/vaults.

Now, with Pulumi ESC, it's time to pick this up again. Pulumi ESC enables us to compose environments, which means we can throw away the stack references.  With Pulumi ESC, we can also expose configuration and secrets as environment variables by running `esc run <environment> -- <command>`; more on that below.

Let's dig into how we did this! _(Note I'm using `pulumi env` here, as support for Pulumi ESC is built into the `pulumi` CLI. There's also a separate CLI tool, `esc`, that can be used with Pulumi ESC.)_

```shell
pulumi env init bjerk/common
```

This command creates an empty environment. You can easily list all the environments you have in your account as such:

```shell
$ pulumi env ls

bjerk/common
```

Next, we wanted to wire up Google Secrets Manager. The nice thing is that we're using [Workload Identity federation][link-5]---which I'm not going to go into right now---and that means we've given Pulumi Cloud access to create a short-lived access token to a service account.

Our basic example looks like this:

```yaml
values:
  gcp:
      login:
        fn::open::gcp-login:
          project: 123456789
          oidc:
            workloadPoolId: esc321
            providerId: esc123
            serviceAccount: esc@on-bjerk.iam.gserviceaccount.com
      secrets:
        fn::open::gcp-secrets:
          login: ${gcp.login}
          access:
            bot-github-token:
              name: bot-github-token
  pulumiConfig:
       github:token: ${gcp.bot-github-token}
```

We are wiring up `gcp` with a service account, provider ID, and workload ID. This service account has access to our secrets, such as the one I'm referencing, `bot-github-token`. We are referencing this secret in `pulumiConfig`, which will be available to Pulumi stacks referencing this environment during `pulumi up/preview/refresh/destroy`.

To evaluate if our setup works, we can run:

```shell
$ pulumi env open bjerk/common

{
  "pulumiConfig": {
    "github": "not-a-secret"
  }
}
```

Jumping back to a Pulumi project, we can easily import this environment to our project by adding this to our Pulumi stack configuration file (`Pulumi.<stack-name>.yaml`).

```yaml
environment:
 imports:
 - bjerk/common
```

## Exposing environment variables

Using our example from above, we can also add the `environmentVariable` keyword to allow developers in our team to use secrets defined in environments.

```yaml
values:
  // ...
  environmentVariables:
    GITHUB_TOKEN: ${gcp.bot-github-token}
```

A particularly nice feature is that we can access these environments with the  `env run`  command.

```shell
$ pulumi env run bjerk/common -- printenv
...
GITHUB_TOKEN=a-secret
```

## What's next?

We are eager to see how Pulumi ESC will evolve. It has already improved a lot of our secrets handling!

One thing I hope to see is a password manager as a provider. At Bjerk, we've learned that storing some secrets in a typical password manager is practical, such as the API keys that we cannot programmatically renew.

I imagine being able to hook up Bitwarden or 1Password in the future with something like this:

```yaml
values:
  gcp:
  // ...
  
  bitwarden:
    login:
      fn::open::not-supported:bitwarden:
        url: https://vaulty.no
        access-token: not-a-secret
    secrets:
      fn::open::not-supported:bitwarden-secrets:
        login: ${bitwarden.login}
        access:
          pat-github-token: simenandre-github-token

  environmentVariables:
    GITHUB_TOKEN: ${bitwarden.pat-github-token}

  pulumiConfig:
       github:token: ${gcp.bot-github-token}
```

As I mentioned above, we can compose multiple providers in one environment. You can see an example here, where we are hooked up to GCP and Bitwarden.

To summarize, Pulumi ESC lets us move secrets to where they belong, enhancing our workflow's security without complicating things for our team.

![A profile picture of Simen Olsen, the author of this post](/public/img/simen-olsen-profile.jpg)

_This article was written by Simen A. W. Olsen. Simen is the manager at Bjerk (a digital product developer agency), a member of the Puluminaries community champion program, and a long-time contributor to Pulumi. He is driven by creating an impact where people feel safe, fulfilled, and empowered to be their best._

[link-1]: https://www.pulumi.com/esc
[link-2]: https://github.com/simenandre/rotate-pulumi-secret
[link-3]: https://github.com/simenandre/get-pulumi-secret
[link-4]: https://www.pulumi.com/registry/packages/gcp/api-docs/secretmanager/getsecret/
[link-5]: https://cloud.google.com/iam/docs/workload-identity-federation
