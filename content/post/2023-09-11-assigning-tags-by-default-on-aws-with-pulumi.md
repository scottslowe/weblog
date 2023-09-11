---
author: slowe
categories: Explanation
comments: true
date: 2023-09-11T09:00:00-04:00
tags:
- AWS
- Go
- IaC
- Pulumi
title: Assigning Tags by Default on AWS with Pulumi
url: /2023/09/11/assigning-tags-by-default-on-aws-with-pulumi/
---

Appropriately tagging resources on [AWS][link-2] is an important part of effectively managing infrastructure resources for many organizations. As such, an infrastructure as code (IaC) solution for AWS must have the ability to ensure that resources are always created with the appropriate tags. (Note that this is subtly different from a policy mechanism that prevents resources from being created without the appropriate tags.) In this post, I'll show you a couple of ways to assign tags by default when creating AWS resources with [Pulumi][link-3]. Code examples are provided in [Golang][link-4].<!--more-->

There are at least two ways (perhaps more) of handling this with Pulumi:

1. Adding the default tags to the stack configuration
2. Adding the default tags to an explicit provider

Each approach has its advantages and disadvantages, so there isn't---in my opinion, at least---a definitive "best way" to doing this. The best way _for you_ will depend on your specific circumstances.

In both cases, the solution involves modifying the configuration of the resource provider Pulumi uses to provision AWS resources. Pulumi supports the notion of both _default providers_ and _explicit providers_. The former are created automatically and are configured via the stack configuration. (In fact, using stack configuration is currently the _only_ way to modify a default provider---see [this GitHub issue][link-1] for more details.) The latter are defined within your Pulumi program, and therefore can be programmatically configured.

With that information in mind, what this really comes down to is:

1. Configuring the default provider via stack configuration to supply default tags
2. Creating an explicit provider in your Pulumi program that has default tags configured

Let's take a look at each approach.

## Using the Stack Configuration

Using the stack configuration to supply default tags is easy and is language-independent; you just modify your `Pulumi.<stack>.yaml` to include the following (substituting your own keys and values, of course):

```yaml
config:
  aws:defaultTags:
    tags:
      Environment: "testing"
      Owner: "slowe"
      Team: "DevRel"
```

When you run `pulumi up` to instantiate your resources, the default AWS provider will automatically add the tags specified in your configuration to taggable resources. That's it.

The advantage of this approach is that it is easy. The disadvantage of this approach is that these values are "static," meaning you can't dynamically derive values here; these are set using `pulumi config set` before you run `pulumi up`. If you wanted to include the stack name in the set of default tags, you'd have to manually set that in the stack configuration for each stack (same goes for project name or Pulumi Cloud organization name, for example). The same goes for _any_ information you'd like to dynamically include.

If you want the flexibility to dynamically include information, then you're better off using an explicit provider.

## Using an Explicit Provider

As I mentioned earlier, an explicit provider is one you create programmatically in your Pulumi program, like this (note that I've intentionally omitted some code for brevity):

```go
explicitAwsProvider, err := aws.NewProvider(ctx, "explicit-aws-provider", ...)
```

Specifically, to create an explicit provider for AWS and specify default tags, it would look something like this in Go:

```go
awsProviderTagged, err := aws.NewProvider(ctx, "aws-provider-tagged", &aws.ProviderArgs{
	Region: pulumi.String("us-west-2"),
	DefaultTags: &aws.ProviderDefaultTagsArgs{
		Tags: pulumi.StringMap{
			"Project":      pulumi.String(ctx.Project()),
			"Stack":        pulumi.String(ctx.Stack()),
			"Organization": pulumi.String(ctx.Organization()),
		},
	},
})
if err != nil {
	return err
}
```

The code above, in addition to providing a rough idea of the syntax to programmatically create an explicit AWS provider with default tags configured, also illustrates how to include dynamic values in the provider configuration. Here you can see using `ctx.Project()`, `ctx.Stack()`, and `ctx.Organization()` to programmatically use the project name, stack name, and organization name (Pulumi Cloud only) as default tags in the explicit AWS provider. This is a key advantage over the configuration-based approach, which doesn't provide a mechanism for using dynamic values.

The drawback to using an explicit provider is that you then need to tell Pulumi to use this explicit provider when creating a resource. This snippet of Go code shows what that looks like:

```go
// Create an S3 bucket; this one will not have default tags
untaggedBucket, err := s3.NewBucket(ctx, "untagged-bucket", nil)
if err != nil {
	return err
}

// Create another S3 bucket; this one will have default tags
taggedBucket, err := s3.NewBucket(ctx, "tagged-bucket", nil, pulumi.Provider(awsProviderTagged))
if err != nil {
	return err
}
```

The first resource doesn't specify a provider, and will therefore use the default provider. Assuming you have _not_ specified `aws:defaultTags` in the stack configuration, this bucket will end up having no tags (you can verify this by using `aws s3api get-bucket-tagging`, as described [in the AWS CLI docs][link-5]). The second resource does specify the explicit provider created earlier, and therefore will have those tags defined on the resource (again, you can verify using the AWS CLI).

Which approach is best? That's really determined by your requirements. Further, it's not really an "either-or" situation; you may want to use the configuration-based approach for the default provider _and_ use an explicit provider (perhaps you need to create resources in two different AWS regions in the same Pulumi program). Each approach has some advantages and disadvantages:

* Specifying `aws:defaultTags` in the stack configuration to configure the default provider is easy and language-independent. However, it lacks the ability to dynamically determine values to use.
* Using an explicit provider provides greater flexibility, but it does create the added work of having to explicitly specify the provider (it's called an explicit provider for a reason) when creating resources.

If you're interested in using this functionality yourself, note that I tested the approaches described above on my macOS 13.5.2 system running Pulumi 3.76.1 and Go 1.21.0 with version 6.0.4 of the AWS provider. However, as far as I am aware, nothing that I've described in this post is specific to any of these versions.

I hope that you find this information helpful. Feel free to join [the Pulumi Community Slack][link-6] if you have additional questions about Pulumi; alternately, I am happy to do my best to help if you reach out to me directly ([Twitter][link-7], [Mastodon][link-8], [Bluesky][link-9]). Thanks for reading!

[link-1]: https://github.com/pulumi/pulumi/issues/6961
[link-2]: https://aws.amazon.com/
[link-3]: https://www.pulumi.com/
[link-4]: https://go.dev/
[link-5]: https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3api/get-bucket-tagging.html
[link-6]: https://slack.pulumi.com/
[link-7]: https://twitter.com/scott_lowe
[link-8]: https://fosstodon.org/@scottslowe
[link-9]: https://bsky.app/profile/scottslowe.bsky.social
