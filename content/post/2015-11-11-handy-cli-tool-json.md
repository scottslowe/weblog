---
author: slowe
categories: Education
comments: true
date: 2015-11-11T00:00:00Z
aliases: /2015/11/20/handy-cli-tool-json/
tags:
- JSON
- OSS
- CLI
title: A Handy CLI Tool for Working with JSON
url: /2015/11/11/handy-cli-tool-json/
---

While I was at [Kubecon][link-2] this past week, one of the presenters showed off a handy CLI tool for working with JSON. It's called [`jq`][link-1], and in this post I'm going to show you a few ways to use `jq`. For the source of JSON output, I'll use the [OpenStack][link-3] APIs.

If you're not familiar with JSON, I suggest having a look at [this non-programmer's introduction to JSON][xref-1]. Also, refer to this article on [using cURL to interact with a RESTful API][xref-2] for a bit more background on some of the commands I'm going to use in this post.

Let's start by getting an authorization token for OpenStack, using the following `curl` command:

    curl -d '{"auth":{"passwordCredentials":\
    {"username": "admin","password": "secret"},\
    "tenantName": "customer-A"}}' \
    -H "Content-Type: application/json" \
    http://192.168.100.100:5000/v2.0/tokens

This will return a pretty fair amount of JSON in the response, and it presents the first opportunity to use `jq`. Let's say you _only_ wanted the authorization token, and not all the other output. Simply add the following `jq` command to the end of your `curl` request:

    curl -d '{"auth":{"passwordCredentials":\
    {"username": "admin","password": "secret"},\
    "tenantName": "customer-A"}}' \
    -H "Content-Type: application/json" \
    http://192.168.100.100:5000/v2.0/tokens | \
    jq '.[].token.id'

This is just the tip of the iceberg for `jq`, though. Now that you have a token, let's get the list of tenants:

    curl -H "X-Auth-Token: <Value from previous command>" \
    -H "Content-Type: application/json" \
    http://192.168.100.100:5000/v2.0/tenants

This gives you another opportunity show off what `jq` can do. We're primarily only interested in the name and ID of the tenants, so you can use `jq` to "reformat" the JSON output from OpenStack to include only that information. That's done with this snippet:

    curl -H "X-Auth-Token: <Value from previous command>" \
    -H "Content-Type: application/json" \
    http://192.168.100.100:5000/v2.0/tenants | \
    jq '.tenants[] | {name: .name, uuid: .id}'

This will provide output that will look something like this:

```json
{
  "name": "customer-A",
  "uuid": "2c9dfbd9f1be3e88c56c88e1e6f38b85"
}
{
  "name": "customer-B",
  "uuid": "e7c59d569a894d42943417f7b6d2931a"
}
```

This makes it really easy to parse out the specific information you need.

Now that you're armed with both an authorization token and a tenant ID, you can ask OpenStack to list the instances belonging to a tenant:

    curl -H "X-Auth-Token: <Value from previous command>" \
    -H "Content-Type: application/json"
    http://192.168.100.100:8774/v2/<Tenant ID>/servers

And once again, we can use `jq` to easily pull out just the information we want:

    curl -H "X-Auth-Token: <Value from previous command>" \
    -H "Content-Type: application/json" \
    http://192.168.100.100:8774/v2/<Tenant ID>/servers | \
    jq '.servers[] | {name: .name, uuid: .id}'

This will pull out only the name and ID of the servers returned by the API call. Using one of the IDs returned by the call, you can then gather information about that instance, and use `jq` to format (pretty print) the resulting output:

    curl -H "X-Auth-Token: <Value from previous command>" \
    -H "Content-Type: application/json" \
    http://192.168.100.100:8774/v2/<Tenant ID>/servers/<server ID> | \
    jq '.'

(For this last use case, you could also use `python -m json.tool`.)

Anyway, there's a ton more that you can do with `jq`, and I'm still discovering all the various uses myself. Be sure to check out [the tutorial][link-4] and [the manual][link-5] for more information.

[link-1]: https://stedolan.github.io/jq/
[link-2]: https://kubecon.io
[link-3]: http://www.openstack.org/
[link-4]: https://stedolan.github.io/jq/tutorial/
[link-5]: https://stedolan.github.io/jq/manual/
[xref-1]: {{< relref "2013-11-08-a-non-programmers-introduction-to-json.md" >}}
[xref-2]: {{< relref "2014-02-19-using-curl-to-interact-with-a-restful-api.md" >}}
