---
author: slowe
categories: Education
comments: true
date: 2014-02-19T16:54:04Z
slug: using-curl-to-interact-with-a-restful-api
tags:
- CLI
- Linux
- NSX
- OpenStack
- Web
- Interoperability
- Networking
- JSON
title: Using Curl to Interact with a RESTful API
url: /2014/02/19/using-curl-to-interact-with-a-restful-api/
wordpress_id: 3404
---

It seems as if APIs are popping up everywhere these days. While this isn't a bad thing, it does mean that IT professionals need to have a better understanding of how to interact with these APIs. In this post, I'm going to discuss how to use the popular command line utility `curl` to interact with a couple of RESTful APIs---specifically, the OpenStack APIs and the VMware NSX API.

Before I go any further, I want to note that to work with the OpenStack and VMware NSX APIs you'll be sending and receiving information in JSON (JavaScript Object Notation). If you aren't familiar with JSON, don't worry---I've have [an introductory post on JSON][1] that will help get you up to speed. (Mac users might also find [this post][2] helpful as well.)

Also, please note that this post is not intended to be a comprehensive reference to the (quite extensive) flexibility of `curl`. My purpose here is to provide enough of a basic reference to get you started. The rest is up to you!

To make consuming this information easier (I hope), I'll break this information down into a series of examples. Let's start with passing some JSON data to a REST API to authenticate.

## Example 1: Authenticating to OpenStack

Let's say you're working with an OpenStack-based cloud, and you need to authenticate to OpenStack using OpenStack Identity ("Keystone"). Keystone uses the idea of tokens, and to obtain a token you have to pass correct credentials. Here's how you would perform that task using `curl`.

You're going to use a couple of different command-line options:

* The "-d" option allows us to pass data to the remote server (in this example, the remote server running OpenStack Identity). We can either embed the data in the command or pass the data using a file; I'll show you both variations.

* The "-H" option allows you to add an HTTP header to the request.

If you want to embed the authentication credentials into the command line, then your command would look something like this:

    curl -d '{"auth":{"passwordCredentials":{"username": "admin",
    "password": "secret"},"tenantName": "customer-A"}}'
    -H "Content-Type: application/json" http://192.168.100.100:5000/v2.0/tokens

I've wrapped the text above for readability, but on the actual command line it would all run together with no breaks. (So don't try to copy and paste, it probably won't work.) You'll naturally want to substitute the correct values for the username, password, tenant, and OpenStack Identity URL.

As you might have surmised by the use of the "-H" header in that command, the authentication data you're passing via the "-d" parameter is actually JSON. (Run it through `python -m json.tool` and see.) Because it's actually JSON, you could just as easily put this information into a file and pass it to the server that way. Let's say you put this information (which you could format for easier readability) into a file named `credentials.json`. Then the command would look something like this (you might need to include the full path to the file):

    curl -d @credentials.json -H "Content-Type: application/json" http://192.168.100.100:35357/v2.0/tokens

What you'll get back from OpenStack---assuming your command is successful---is a wealth of JSON. I highly recommend piping the output through `python -m json.tool` as it can be difficult to read otherwise. (Alternately, you could pipe the output into a file.) Of particular usefulness in the returned JSON is a section that gives you a token ID. Using this token ID, you can prove that you've authenticated to OpenStack, which allows you to run subsequent commands (like listing tenants, users, etc.).

## Example 2: Authenticating to VMware NSX

Not all RESTful APIs handle authentication in the same way. In the previous example, I showed you how to pass some credentials in JSON-encoded format to authenticate. However, some systems use other methods for authentication. VMware NSX is one example.

In this example, you'll need to use a different set of `curl` command-line options:

* The "--insecure" option tells `curl` to ignore HTTPS certificate validation. VMware NSX controllers only listen on HTTPS (not HTTP).

* The "-c" option stores data received by the server (one of the NSX controllers, in this case) into a cookie file. You'll then re-use this data in subsequent commands with the "-b" option.

* The "-X" option allows you to specify the HTTP method, which normally defaults to GET. In this case, you'll use the POST method along the the "-d" parameter you saw earlier to pass authentication data to the NSX controller.

Putting all this together, the command to authenticate to VMware NSX would look something like this (naturally you'd want to substitute the correct username and password where applicable):

    curl --insecure -c cookies.txt -X POST -d 'username=admin&password;=admin' https://192.168.100.50/ws.v1/login

## Example 3: Gathering Information from OpenStack

Once you've gotten an authentication token from OpenStack as I showed you in example #1 above, then you can start using API requests to get information from OpenStack.

For example, let's say you wanted to list the instances for a particular tenant. Once you've authenticated, you'd want to get the ID for the tenant in question, so you'd need to ask OpenStack to give you a list of the tenants (you'll only see the tenants your credentials permit). The command to do that would look something like this:

    curl -H "X-Auth-Token: <Token ID>" http://192.168.100.70:5000/v2.0/tenants

The value to be substituted for token ID in the above command is returned by OpenStack when you authenticate (that's why it's important to pay attention to the data being returned). In this case, the data returned by the command will be a JSON-encoded list of tenants, tenant IDs, and tenant descriptions. From that data, you can get the ID of the tenant for whom you'd like to list the instances, then use a command like this:

    curl -H "X-Auth-Token: <Token ID>" http://192.168.100.70:8774/v2/<Tenant ID>/servers

This will return a stream of JSON-encoded data that includes the list of instances and each instance's unique ID---which you could then use to get more detailed information about that instance:

    curl -H "X-Auth-Token: <Token ID>" http://192.168.100.70:8774/v2/<Tenant ID>/servers/<Server ID>

By and large, the API is reasonably well-documented; you just need to be sure that you are pointing the API call against the right endpoint. For example, authentication has to happen against the server running Keystone, which may or may not be the same server that is running the Nova API services. (In the examples I just provided, Keystone and the Nova API services are running on the same host, which is why the IP address is the same in the command lines.)

## Example 4: Creating Objects in VMware NSX

Getting information from VMware NSX using the RESTful API is very much like what you've already seen in getting information from OpenStack. Of course, the API can also be used to create objects. To create objects---such as logical switches, logical switch ports, or ACLs---you'll use a combination of `curl` options:

* You'll use the "-b" option to pass cookie data (stored when you authenticated to NSX) back for authentication.

* The "-X" option allows you to specify the HTTP method (in this case, POST).

* The "-d" option lets us transfer JSON-encoded data to form the request for the object we are going to create. We'll specify a filename here, preceded by the "@" symbol.

* The "-H" option adds an appropriate "Content-Type: application/json" header to the request, since we are passing JSON-encoded data to the NSX controller.

When you put it all together, it looks something like this (substituting appropriate values where applicable):

    curl --insecure -b cookies.txt -d @new-switch.json 
    -H "Content-Type: application/json" -X POST https://192.168.100.50/ws.v1/lswitch

As I mentioned earlier, you're passing JSON-encoded data to the NSX controller; here are the contents of the `new-switch.json` file referenced in the above command example:

``` json
{
  "display_name": "test-lswitch", 
  "port_isolation_enabled": false, 
  "transport_zones": [
    {
      "zone_uuid": "dca3d854-b329-5458-b711-0df2d5762d7a", 
      "binding_config": {}, 
      "transport_type": "stt"
    }
  ], 
  "replication_mode": "source", 
  "type": "LogicalSwitchConfig"
}
```

(Click [here](https://gist.github.com/scottslowe/9103770) for this JSON data as a GitHub gist.)

Once again, I recommend piping the output of this command through `python -m json.tool`, as what you'll get back on a successful call is some useful JSON data that includes, among other things, the UUID of the object (logical switch, in this case) that you just created. You can use this UUID in subsequent API calls to list properties, change properties, add logical switch ports, etc.

Clearly, there is _much_ more that can be done with the OpenStack and VMware NSX APIs, but this at least should give you a starting point from which you can continue to explore in more detail. If anyone has any corrections, clarifications, or questions, please feel free to post them in the comments section below. All courteous comments (with vendor disclosure, where applicable) are welcome!

[1]: {{< relref "2013-11-08-a-non-programmers-introduction-to-json.md" >}}
[2]: {{< relref "2013-11-11-making-json-output-more-readable-with-bbedit.md" >}}
