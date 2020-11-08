---
author: slowe
categories: Explanation
comments: true
date: 2020-04-12T07:05:00-07:00
tags:
- AWS
- XML
- macOS
title: Using Paw to Launch an EC2 Instance via API Calls
url: /2020/04/12/using-paw-to-launch-ec2-instance-via-api-calls/
---

Last week I wrote a post on [using Postman to launch an EC2 instance via API calls][xref-1]. [Postman][link-1] is a cross-platform application, so while my post was centered around Postman on Linux ([Ubuntu][link-2], specifically) the steps should be very similar---if not exactly the same---when using Postman on other platforms. Users of macOS, however, have another option: a macOS-specific peer to Postman named [Paw][link-3]. In this post, I'll walk through using Paw to issue API requests to AWS to launch an EC2 instance.<!--more-->

I'll structure this post as a "diff," if you will, that outlines the differences of using Paw to launch an EC2 instance via API calls versus using Postman to do the same thing. Therefore, if you haven't already read [the Postman post from last week][xref-1], I _strongly_ recommend reviewing it before proceeding.

## Prerequisites

This post assumes you've already installed Paw on your macOS system. It also assumes you are somewhat familiar with Paw; refer to the Paw documentation if not. Also, to support AWS authentication, please be sure to install the "AWS Signature 4 Auth Dynamic value" extension (see [here][link-4] or [here][link-5]). This extension is necessary in order to have the API requests sent by Paw properly authenticated to AWS.

Finally, you'll want to ensure you have a valid access key ID and corresponding secret access key; your API requests will use these to authenticate against AWS.

## Similarities Between Paw and Postman

First, let's look at some of the similarities between using these two tools to interact with the AWS APIs:

* Both tools support the use of environments and environment variables, and I'd recommend using this functionality. So, if you're using Paw, be sure to capture your AWS access key ID, AWS secret access key, and AWS region as environment variables you can reference later.
* The query parameters and the API endpoint will be the same for both products (you are, after all, interacting with the same API). You can use the region environment variable in your request URL in both products, although the way in which you access it will be slightly different with Paw (more on that below).

More important than the similarities, though, are the differences between the two products, and how that affects using these products to interact with the AWS APIs.

## Differences Between Paw and Postman

Having reviewed the similarities, let's now look at some of the differences:

* For proper authentication to the AWS APIs, you'll need to install a Paw extension, as outlined in the "Prerequisites" section.
* Paw doesn't appear to have the equivalent of a Collection object that can hold configuration settings, so you'll have to configure the correct headers for authentication for each request individually. See the "Configuring Headers for the Requests" section below for details.
* Paw doesn't have the concept of tests as in Postman, so there's no need to write custom JavaScript to capture values from API responses (in order to chain requests). Instead, Paw just "automatically" makes response values available. See the "Referencing Response Values" section below.

The next couple of sections look at some of the differences in more detail.

### Configuring Headers for the Requests

Authentication for API requests to the AWS APIs are handled via two headers sent with the API requests: the Authorization header, and the X-Amz-Date header. In Paw, you will need to configure these headers on each API request individually; you can't apply them to a Collection-type object in Paw like you can in Postman.

Here's a screenshot of configuring the headers for the API requests:

![Configuring headers in Paw](/public/img/paw-headers.png)

The Authorization header is configured to use the AWS Signature 4 Auth extension (selected by right-clicking in the Header Value field and selecting Extensions &gt; AWS Signature v4 Auth), while the X-Amz-Date is configured to use an automatic timestamp (inserted by right-clicking and going to Values &gt; Timestamp &gt; Custom Formatting). The correct timestamp format to use is what's shown in the screenshot (`%G%m%dT%H%M%SZ`).

When you add the AWS Signature v4 Auth extension, you'll also need to configure it. Here's a screenshot of the configuration of the extension; you can see it's using environment variables to provide the AWS access key ID, AWS secret access key, and AWS region:

![Configuring the AWS Signature v4 Auth extension](/public/img/paw-extension-config.png)

Remember that these headers need to configured for each request, or the API request won't authenticate properly.

### Referencing Response Values

The other major difference between Postman and Paw is in how each program allows you to chain API requests together using values from previous API requests. In Postman, this is handled using snippets of JavaScript the user has to write. In Paw, this is handled somewhat "automatically"; the user only needs to insert what's called a "dynamic repsonse value," and then reference the API request and the field/value from the response.

Here's a screenshot of the API request to run an instance, which---as you'll recall from the Postman post---needs several values from earlier API requests:

![Referencing earlier response values](/public/img/paw-reference-response-values.png)

These response values are inserted by right-clicking in the field where they should go and then selecting Response &gt; Filtered Response Body. This brings up a dialog box to configure it, where you'll select the API request, the response format (XML in this case), and then provide the XML path to the value you want. Here's an example screenshot:

![Configuring a response value](/public/img/paw-config-response-value.png)

That's it---aside from these differences in having to configure headers on each API request and in referencing values from earlier responses, the process for interacting with the AWS APIs using Paw is extremely similar to using Postman. For the purposes of launching an EC2 instance via APIs, the requests and the values needed from each request remain exactly the same.

I hope this brief walkthroug/comparison of using Paw to interact with the AWS APIs is useful. If you have questions, want to provide feedback, or if you've found an error in my post, I'd love to hear from you. Hit [me on Twitter][link-99], or track down my email (it's not hard) and send me a message. Thanks!

[link-1]: https://www.postman.com/
[link-2]: https://ubuntu.com/
[link-3]: https://paw.cloud/
[link-4]: https://github.com/badslug/Paw-AWSSignature4DynamicValue
[link-5]: https://paw.cloud/extensions/
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2020-04-10-using-postman-to-launch-ec2-instance-via-api-calls.md" >}}
