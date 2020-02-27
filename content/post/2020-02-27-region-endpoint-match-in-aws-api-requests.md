---
author: slowe
categories: Explanation
comments: true
date: 2020-02-27T09:45:00-07:00
tags:
- AWS
title: Region and Endpoint Match in AWS API Requests
url: /2020/02/27/region-endpoint-match-in-aws-api-requests/
---

Interacting directly with the AWS APIs---using a tool like [Postman][link-2] (or, since I switched back to macOS, an application named [Paw][link-1])---is something I've been doing off and on for a little while as a way of gaining a slightly deeper understanding of the APIs that tools like [Terraform][link-4], [Pulumi][link-5], and others are calling when automating AWS. For a while, I struggled with AWS authentication, and after seeing Mark Brookfield's post on [using Postman to authenticate to AWS][link-3] I thought it might be helpful to share what I learned as well.<!--more-->

The basis of Mark's post (I highly encourage you to go read it) is that he was having a hard time getting authenticated to AWS in order to automate the creation of some Route 53 DNS records. The root of his issue, as it turns out, was a mismatch between the region specified in his request and the API endpoint for Route 53. I know this because I ran into the exact same issue (although with a different service).

The secret to uncovering this mismatch can be found in [this "AWS General Reference" PDF][link-6]. Specifically with regard to Route 53, check out this quote from the document:

>When you submit requests using the AWS CLI or SDKs, either leave the Region and endpoint unspecified, or specify us-east-1 as the Region. When you submit requests using the Route 53 API, use the us-east-1 Region to sign requests.

Also note that the list of API endpoints further down the page show that for all regions, the API endpoint is simply "route53.amazonaws.com".

This directly mirrors Mark's experience---even though he was working in the "eu-west-1" region, he had to use "us-east-1" as the region for authentication and the API endpoint was not region-specific.

Now let's compare that to working with the EC2 API (see page 130 of the linked general reference PDF). Note that it does not provide any guidance like it did for Route 53, and note that region-specific API endpoints are listed. In cases like this---and this is where I was getting tripped up for a while in my efforts---_the region specified for signing the authentication request must match the region specified in the API endpoint._

Here's an example from my own experiments. The screenshots below are taken from Paw, a macOS-native peer to Postman.

If you want to follow along and try something like this yourself, you'll need to first install the "AWS Signature 4 Auth Dynamic Value" extension (Paw extensions are available [here][link-7]):

![Paw extensions](/public/img/paw-auth-extension.jpg "Paw AWS Signature 4 Auth extension")

Once the extension is installed, you can create a request. Add an Authorization header, and specify the AWS Signature 4 Auth extension as the value. Then configure the extension with the right values:

![AWS Signature 4 Auth extension configuration](/public/img/paw-endpoint-auth-region-match.jpg "Configuring the extension")

(I'm using environment variables here to provide my access key and secret key.)

The key takeaway here is to note that the API endpoint (`https://ec2.us-west-2.amazonaws.com`) for the request matches the region specified in the extension configuration---and it's the value specified in the extension configuration that's used to sign the authentication request. If these two values are mismatched---for example, if the request is sent to `https://ec2.amazonaws.com` but the signing region is set to "us-west-2", then the authentication will fail. (You can follow the discussion about the errors I was seeing on [this GitHub issue][link-8].)

Knowing when you need to use a region-specific endpoint (and thus match the region in the request) and when you can/should use a generic endpoint is where [the general reference PDF][link-6] comes in. Unfortunately, for other services that use a generic API endpoint---like IAM---it's not clear if you should leave the region blank or specify "us-east-1" (it's not called out like it is for Route 53). A quick test with Paw shows that IAM calls should be handled like Route 53 calls, but it would be great if the API documentation was updated to reflect this (I also took a look at [the IAM API reference][link-9] to see if it was called out there, but didn't find it).

Anyway, this is all just a long-winded way of saying that for AWS services that have region-specific API endpoints you'll need to be sure to use a matching region in the Authorization header; for AWS services that have a generic API endpoint (like Route 53 or IAM), you'll most likely need to either leave the region blank or specify "us-east-1".

Hit me up [on Twitter][link-10] if you have more information, additional resources that other readers may find useful, or if something I've said above needs to be corrected. (You can also just say hi if you'd like.)

[link-1]: https://paw.cloud/
[link-2]: https://www.postman.com/
[link-3]: https://virtualhobbit.com/2020/02/26/wednesday-tidbit-using-postman-to-authenticate-to-aws/
[link-4]: https://www.terraform.io/
[link-5]: https://www.pulumi.com/
[link-6]: https://docs.aws.amazon.com/general/latest/gr/aws-general.pdf
[link-7]: https://paw.cloud/extensions/
[link-8]: https://github.com/badslug/Paw-AWSSignature4DynamicValue/issues/20
[link-9]: https://docs.aws.amazon.com/IAM/latest/APIReference/iam-api.pdf
[link-10]: https://twitter.com/scott_lowe
