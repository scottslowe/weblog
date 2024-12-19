---
author: slowe
categories: Explanation
comments: true
date: 2024-12-19T12:00:00-06:00
tags:
- AWS
- IaC
- Pulumi
title: Using Multiple AWS Regions with Pulumi and S3 Backend
url: /2024/12/19/using-multiple-aws-regions-with-pulumi-and-s3-backend/
---

For a while now, I've been using [Direnv][link-1] to manage environment variables when I enter or leave certain directories. Since I have to work with more than one AWS account, one of the use cases for me has been populating AWS-specific environment variables, like `AWS_REGION` or `AWS_PROFILE`. This generally works really well for me, but recently I ran into a bit of a corner case involving multiple AWS regions, [Pulumi][link-2], and using S3 as the Pulumi backend. In this post, I'll share the workaround that allows this configuration to work as expected.<!--more-->

I describe this as a "bit of a corner case" because it only affects specific configurations (which included my configuration):

* You must be setting the `AWS_REGION` environment variable and _not_ setting the `aws:region` configuration value used by the Pulumi AWS provider.
* You must be using S3 as the backend for Pulumi, and using an S3 URL of `s3://bucket-name`.
* You want to deploy resources into an AWS region that is _different_ than the AWS region where the backend state bucket resides.

In my specific situation, my backend state bucket resides in the AWS us-west-2 (Oregon) region, as this offers the lowest latencies from my home office in Colorado. I control this via the `PULUMI_BACKEND_URL` environment variable using the `s3://bucket-name` syntax. I needed to deploy resources into the us-east-2 (Ohio) region, so I set the `AWS_REGION` environment variable accordingly. Because I used `AWS_REGION`, I did not set the `aws:region` Pulumi provider configuration value.

When I ran `pulumi stack init` to create a Pulumi stack for the Ohio resources, I got an error message containing the text "incorrect region, the bucket is not in 'us-east-2' region at endpoint '', bucket is in 'us-west-2' region".

After a bit of Googling, I realized the problem: Pulumi was looking for the backend state bucket in the region specified by `AWS_REGION`. How, then, does one deploy to one AWS region when needing to use a backend state bucket in a different region?

The answer sits buried in [this section of the Pulumi documentation][link-3]; specifically, in this callout (emphasis mine):

> As of Pulumi CLI v3.33.1, instead of specifying the AWS Profile, add `awssdk=v2` along **with the region** and profile to the query string.

Ah ha! All that's required is to amend the `PULUMI_BACKEND_URL` setting to look something more like this:

```shell
s3://bucket-name?region=us-west-2&awssdk=v2&profile=aws-profile
```

That worked perfectly---Pulumi now knew to look for the backend state bucket in the us-west-2 (Oregon) region while deploying resources to the region specified by the `AWS_REGION` environment variable. In theory, since I didn't need to use different AWS profiles for the backend state bucket versus where resources live, it might be possible to remove `&profile=aws-profile` from the S3 URL. I haven't tested that yet.

There you have it---to use different AWS regions for your S3 backend state bucket and the resources you're deploying, make sure your `PULUMI_BACKEND_URL` setting properly includes the region in the URL as described above.

I hope this helps someone else out there! Hit me up [on Twitter][link-4], [on Mastodon][link-5], or [on Bluesky][link-6] if you have any feedback or any questions.

[link-1]: https://direnv.net/
[link-2]: https://www.pulumi.com/
[link-3]: https://www.pulumi.com/docs/iac/concepts/state-and-backends/#aws-s3
[link-4]: https://twitter.com/scott_lowe
[link-5]: https://fosstodon.org/@scottslowe
[link-6]: https://bsky.app/profile/scottslowe.bsky.social
