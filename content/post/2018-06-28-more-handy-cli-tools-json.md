---
author: slowe
categories: Education
comments: true
date: 2018-06-28T08:00:00Z
tags:
- JSON
- CLI
- AWS
- Azure
title: More Handy CLI Tools for JSON
url: /2018/06/28/more-handy-cli-tools-json/
---

In late 2015 I wrote [a post about a command-line tool named `jq`][xref-2], which is used for parsing JSON data. Since that time I've referenced `jq` in a number of different blog posts (like [this one][xref-3]). However, `jq` is not the only game in town for parsing JSON data at the command line. In this post, I'll share a couple more handy CLI tools for working with JSON data.<!--more-->

(By the way, if you're new to JSON, check out [this post][xref-1] for a gentle introduction.)

## JMESPath and `jp`

[JMESPath][link-2] is used by both Amazon Web Services (AWS) in their AWS CLI as well as [by Microsoft in the Azure CLI][link-1]. For examples of JMESPath in action, see [the AWS CLI documentation on the `--query` functionality][link-5], which makes use of server-side JMESPath queries to reduce the amount of data returned by an AWS CLI command (as opposed to filtering on the client side).

However, you can also use JMESPath on the client-side through [the `jp` command-line utility][link-4]. As a client-side parsing tool, `jp` is similar in behavior to `jq`, but I find the JMESPath query language to be a bit easier to use than `jq` in some situations.

Let's assume we are working with the output of the command `aws ec2 describe-security-groups`, which returns---as JSON---a list of security groups and their properties. Naturally, parsing this data down to find only the specific information you need is a prime use case for `jp`. Perhaps you know that a security group named "internal-only" exists, but you don't know anything else---only the name. Using `jp`, we could get more details on that specific group with this command:

    aws ec2 describe-security-groups | jp "SecurityGroups[?GroupName == 'internal-only']"

The AWSCLI command will return all the security groups, and `jp` will filter through the data to return only the properties of the security group whose name (as defined in the "GroupName" field/property) is equal to "internal-only." Compare that syntax to the equivalent `jq` syntax:

    aws ec2 describe-security-groups | jq '.SecurityGroups[] | select (.GroupName == "internal-only")'

That's handy, but what if we needed only a particular property of that security group? No problem, we'd just append the property name to the end of the query:

    aws ec2 describe-security-groups | jp "SecurityGroups[?GroupName == 'internal-only'].GroupId"

This is fundamentally equivalent to this `jq` command:

    aws ec2 describe-security-groups | jq '.SecurityGroups[] | select (.GroupName == "internal-only").GroupId'

However, if you try both `jp` and `jq` as described above, you'll note a significant difference in the output. First, here's the output of a `jq` command like the one above:

``` text
"sg-7f7efe02"
```

Now compare that to the output of the equivalent `jp` command:

``` text
[
  "sg-7f7efe02"
]
```

As you can see, `jq` returns a specific value, whereas `jp` returns an array of values (with only a single item in the array). In order to get all the way down to a single value with `jp`, you have to extend the query:

    aws ec2 describe-security-groups | jp "SecurityGroups[?GroupName == 'internal-only'].GroupId | [0]"

This command will return _only_ the value of the "GroupId" field. `jp` does support a `-u` command-line option that is equivalent to the `-r` option to `jq` in order to return unquoted (raw) strings. This is helpful when storing the command output into a variable for use later.

If you need more than a single property, you can build a JSON object from multiple properties by listing them in curly braces at the end of the query, like this:

    aws ec2 describe-security-groups | jp "SecurityGroups[?GroupName == 'internal-only'].{ Name: GroupName, ID: GroupId, VPC: VpcId }"

This will return a JSON object with the properties listed.

I may explore writing a more in-depth article on `jp` in the future; if that's something in which you'd be interested, please [hit me up on Twitter][link-3] and let me know.

## JSON Incremental Digger (`jid`)

What if you're not all that well-versed in the JMESPath syntax? Well, there are online simulators and parsers. Another approach would be to use [`jid`, the JSON incremental digger][link-6]. It doesn't necessarily follow the JMESPath syntax, but it does allow you to interactively query some JSON data to find what you're seeking.

To use `jid`, simply pipe in some JSON data. For example, you could direct the output of the AWS CLI into `jid`:

    aws ec2 describe-instances | jid

This puts you into an interactive screen where you can explore and parse the data to find exactly what you need. `jid` will supply "suggested" queries at the top of the screen in green; just press Tab to accept the suggestion. Adjust the query at the top until you have the data you need, then press Enter. The specific data you'd selected in `jid` will be output to the shell. This is a super-handy way, in my opinion, of exploring JSON data structures with which you aren't already familiar.

Have any other handy CLI tools for working with JSON? [Hit me on Twitter][link-3] with other tool suggestions, and I'll update the post with feedback from readers. Thanks!

[link-1]: https://docs.microsoft.com/en-us/cli/azure/query-azure-cli
[link-2]: http://jmespath.org/
[link-3]: https://twitter.com/scott_lowe
[link-4]: https://github.com/jmespath/jp
[link-5]: https://docs.aws.amazon.com/cli/latest/userguide/controlling-output.html
[link-6]: https://github.com/simeji/jid
[xref-1]: {{< relref "2013-11-08-a-non-programmers-introduction-to-json.md" >}}
[xref-2]: {{< relref "2015-11-11-handy-cli-tool-json.md" >}}
[xref-3]: {{< relref "2018-05-23-quick-post-parsing-aws-instance-data-with-jq.md" >}}
