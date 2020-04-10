---
author: slowe
categories: Explanation
comments: true
date: 2020-04-10T15:30:00-07:00
tags:
- AWS
- XML
- Linux
title: Using Postman to Launch an EC2 Instance via API Calls
url: /2020/04/10/using-postman-to-launch-ec2-instance-via-api-calls/
---

As I mentioned in this post on [region and endpoint match in AWS API requests][xref-1], exploring the AWS APIs is something I've been doing off and on for several months. There's a couple reasons for this; I'll go into those in a bit more detail shortly. In any case, I've been exploring the APIs using [Postman][link-3] (when on Linux) and [Paw][link-4] (when on macOS), and in this post I'll share how to use Postman to launch an EC2 instance via API calls.<!--more-->

Before I get into the technical details, let me lay out a couple reasons for spending some time on this. I'm pretty familiar with tools like [Terraform][link-1] and [Pulumi][link-2] (my current favorite), and I'm reasonably familiar with AWS CLI itself. In looking at working directly with the APIs, I see this as adding a new perspective on how these other tools work. (I've  found, in fact, that exploring the APIs has improved my usage of the AWS CLI.) Finally, as I try to deepen my knowledge of programming languages, I wanted to have a reasonable knowledge of the APIs before trying to program around the APIs (hopefully this will make the learning curve a bit less steep).

## Prerequisites

This post assumes you've already installed Postman, so I won't be covering that here (for a rough idea of what that process looks like, see [here][xref-2]). I'll be using Linux, but since Postman is cross-platform then this post should cover most users. If anyone is interested in a version of this post with Paw (a macOS-specific API client), contact [me on Twitter][link-99] and let me know. This post also assumes you're somewhat familiar with Postman; if that's not the case, check out [the Postman documentation][link-7].

You'll want to ensure that you have a valid access key ID and corresponding secret access key; your API requests will use these to authenticate against AWS.

## Set up the Postman Environment

To simplify things, the first step I'd recommend is setting up a Postman environment. In this environment, you'll initially want to store three pieces of information:

1. The value of your AWS access key ID
2. The value of your AWS secret access key
3. The AWS region with which you'll be working

Why put these values into a Postman environment? Well, there are times when I need to flip between personal and work accounts. By placing these values into separate environments with the same variable names, I can switch between accounts with no changes to the actual API requests.

If you're concerned about the security of your account(s)---which is reasonable---create a dedicated IAM user with its own acces key ID and secret access key (assuming you have permissions in your account to do so), and store those values in the Postman environment.

## Set up the Collection

In Postman, a collection is more than just a folder for collecting API requests. At the collection level, you can also set certain configuration values. In this case, I'd recommend at least setting up the Authorization settings for the collection, and these settings will then trickle down to all API requests in the collection.

Set your Authorization value to "AWS Signature," and then use the environment variables you created in the previous section to reference the values of your access key ID and secret access key. Under the "Advanced" section, reference your region using another environment variable, and specify the Service Name as "ec2". The final configuration should look something like this (the names of your environment variables may be different):

![Configuring Authorization parameters on the Collection](/public/img/postman-authz-collection.png)

Once the Collection is configured, and you have an environment in place with the right values, you're ready to create the API requests.

## Craft the API Requests

Before you can launch an instance, you'll first need to craft some API calls to gather the information needed to launch an instance. Specifically, you'll need a subnet ID, a key pair name, and an AMI ID. You _could_ hard-code these values into an API request to launch an instance, but that won't help much (in my opinion) with expanding one's knowledge of how the APIs work.

You'll need five API requests to gather information first:

1. An API request to retrieve the Availability Zones (AZs) for the region you've specified
2. An API request to get the default VPC for your account in the region you've specified
3. An API request to retrieve the list of SSH key pairs in your account in the region you've specified
4. An API request to get the default subnet in the default VPC for an available AZs in the region you've specified
5. An API request to retrieve the image ID for the desired Amazon Machine Image (AMI) you want to use

To help in crafting these API calls, I found [the AWS EC2 API reference][link-8] to be quite handy. For each of these five API calls to gather information, you'll use a corresponding _Action_ as a parameter to the API call:

1. To retrieve a list of AZs, the Action is DescribeAvailabilityZones
2. To get the default VPC, the Action is DescribeVpcs
3. For the list of key pairs, you'll use the DescribeKeyPairs Action
4. To get the default subnet in a VPC in a particular AZ, the action is DescribeSubnets (seeing a pattern yet?)
5. To determine the image ID for the desired AMI, you'll use the Action DescribeImages

Each of these Actions will share a common API endpoint, which is what you'll specify as the URL for the API request. If you included AWS region as a variable in your environment, then you can reference that region in the API endpoint to make your API requests more region-independent, like this:

    https://ec2.{{region}}.amazonaws.com/

Each of these Actions will also share one common parameter---the Version parameter, with a value of "2016-11-15". Otherwise, the parameters for each Action (each API call) will be different. Let's take a look at those.

### Getting the List of AZs

Here's a screenshot of the query parameters for the first of the five API calls, the one to retrieve the list of AZs:

![Query parameters for retrieving a list of AZs](/public/img/describe-az-query-params.png)

You'll note that Postman will report that eight Headers have been added to your request; these were all added by configuring the Authorization setting on the Collection. No other changes should be necessary in order for this request to work. When you send this request, you'll get back some XML (not JSON) with the results of your query.

In order to be able to use some part of this response later---which you'll need to do, unless you want to hard-code values---you'll also need to write a small JavaScript "test" that captures some of this information into your Postman environment. Here's the JavaScript you'd need to write to capture the name of the first AZ returned by the API call:

```javascript
var jsonObject = xml2Json(pm.response.text());
pm.environment.set("firstAz", jsonObject.DescribeAvailabilityZonesResponse.availabilityZoneInfo.item[0].zoneName);
```

To make it easier for folks to follow along, I've added the necessary JavaScript for each of the five API queries to [my GitHub "learning-tools" repository][link-9]. See the `aws/postman-aws-api` folder. For the AZ query, the corresponding JavaScript file is named `describe-azs-test.js`, and it stores the name of the first AZ returned by the query into the Postman environment as a variable named "firstAz".

### Determining the Default VPC

This API request will differ only from the first one in the parameters. It will use the same API endpoint, and will have the same Version parameter. Here's a screenshot of the parameters for this API request:

![Retrieving the default VPC](/public/img/describe-vpcs-query-params.png)

As you can see in the screenshot, you'll need to use two additional parameters. These parameters correspond to the `--filters` argument of the AWS CLI, and you'll use them to tell AWS to return _only_ the default VPC. The values to use here are described in [the AWS CLI reference][link-10].

The value returned by this request does need to be captured using a JavaScript test (configured on the Tests tab of the Postman request). I have a sample JavaScript file named `describe-vpcs-test.js` you can use as an example (see previous section for the location of the sample JavaScript test files).

### Getting the Key Pair

The API request to get the SSH key pair name is very straightforward. Here's a screenshot of the query parameters:

![Getting the SSH key pair name](/public/img/describe-key-pairs-query-params.png)

To capture a value out of the response to this request, you can use the sample JavaScript test in [my GitHub "learning-tools" repository][link-9]; it's named `describe-key-pairs-test.js` in the `aws/postman-aws-api` directory.

### Determing the Subnet to Use

This API request will really demonstrate the reason why the JavaScript tests are needed to capture data out of the API responses, as you'll need some of that information. The purpose of this API request is to determine the default subnet, given a particular VPC and a particular AZ. This means you'll need the VPC ID and AZ name captured by the earlier API requests, and stored into the Postman environment by the JavaScript tests.

Here's a screenshot of the query parameters for this API request:

![Getting the default subnet for a VPC and AZ](/public/img/describe-subnets-query-params.png)

If you changed the names of the environment variables used in the JavaScript tests for getting the list of AZs and getting the default VPC, you'll want to adjust them here to ensure this request works.

Because we'll need the value returned by this call later, you'll also need a JavaScript test for this request. The `describe-subnets-test.js` file in the `aws/postman-aws-api` directory of [my GitHub "learning-tools" repository][link-9] can be used as an example.

### Finding the Image ID

This request, out of all the others, may end up with more query parameters than the others illustrated here---mainly in order to use multiple filter parameters for narrowing down the list of results. For each filter, you'd have two query parameters: Filter.X.Name and Filter.X.Value (these directly correspond to the `Name=,Value=` portions of the `--filters` argument of the AWS CLI).

Here's my example; yours will very likely look different:

![Retrieving the correct image ID](/public/img/describe-images-query-params.png)

In my particular case, I filtered by images I own (using the "Owner.1" query parameter) and by a particular tag name and value. You will likely have to experiment with the right set of filters in order to get the value you want. Once you have the value you want, you'll need a JavaScript test to capture that value for use in the final API request to actually launch an instance. Check the `aws/postman-aws-api` directory in my GitHub "learning-tools" repository for the `describe-images-test.js` file to use as an example.

### Launching the EC2 Instance

It's finally time to send the API request to actually launch an EC2 instance! This time you'll use the RunInstances Action, and you'll use environment variables supplied by several of the JavaScript tests used in earlier API requests. Here's a screenshot of the query parameters for the final RunInstances API request:

![Launching an EC2 instance](/public/img/run-instances-query-params.png)

You'll note from the screenshot there's no JavaScript test associated with this API request. If you want to capture the instance ID of the instance you just spawned with this request, you could add a JavaScript test here to do that, and then be able to use this instance ID in additional API requests (perhaps to query the status of the instance, or to terminate the instance). If you used different environment variables in some of the JavaScript tests in earlier API requests, you'll want to make sure you use the correct variable names here in order for this request to work.

And that's it! Flip over to your AWS Console or fire up the AWS CLI to see the instance you just launched (or do some additional API exploration of your own to gather information about the instance).

## Additional Resources

There are several additional resources that I found extremely helpful while learning some of the concepts discussed in this post. I've included them below for the benefit of others:

[Organizing EC2 API Actions As A Postman Collection][link-5]

[Extracting data from responses and chaining requests][link-6]

In the post above, I also provided links to the AWS EC2 API reference as well as the AWS CLI reference, both of which are also very useful resources. Finally, the Postman documentation was very helpful, and a link for it is also provided above.

If you have any questions about the information presented in this post, I'd love to hear from you. Feel free to contact [me on Twitter][link-99]. I know this was a long post; thanks for hanging in there until the end!

[link-1]: https://www.terraform.io/
[link-2]: https://www.pulumi.com/
[link-3]: https://www.postman.com/
[link-4]: https://paw.cloud
[link-5]: https://apievangelist.com/2019/11/18/organizing-ec2-api-actions-as-a-postman-collection/
[link-6]: https://blog.postman.com/2014/01/27/extracting-data-from-responses-and-chaining-requests/
[link-7]: https://learning.postman.com/docs/postman/launching-postman/introduction/
[link-8]: https://docs.aws.amazon.com/AWSEC2/latest/APIReference/Welcome.html
[link-9]: https://github.com/scottslowe/learning-tools/
[link-10]: https://docs.aws.amazon.com/cli/latest/reference/ec2/index.html
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2020-02-27-region-endpoint-match-in-aws-api-requests.md" >}}
[xref-2]: {{< relref "2017-11-21-installing-postman-fedora-27.md" >}}
