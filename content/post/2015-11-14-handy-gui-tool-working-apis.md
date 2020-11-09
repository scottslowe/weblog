---
author: slowe
categories: Education
comments: true
date: 2015-11-14T00:00:00Z
tags:
- JSON
- macOS
- OpenStack
- Web
- Interoperability
title: A Handy GUI Tool for Working with APIs
url: /2015/11/14/handy-gui-tool-working-apis/
---

In this post I'm going to share with you an OS X graphical application I found that makes it easier to work with RESTful APIs. The topic of RESTful APIs has come up here before (see [this post on using cURL to interact with RESTful APIs][xref-1]), and RESTful APIs have been a key part of a number of other posts (like my [recent post on using `jq` to work with JSON][xref-2]). Unlike these previous posts---which were kind of geeky and focused on the command line---this time around I'm going to show you an application called [Paw][link-1], which provides a graphic interface for working with APIs.

Before I start talking about Paw, allow me to first explain _why_ I'm talking about working with APIs using this application. I firmly believe that the future of "infrastructure engineers"---that is, folks who today are focused on managing servers, hypervisors, VM, storage, networks, and firewalls---lies in becoming the "full-stack engineer," someone who has knowledge and skills across multiple areas, including automation/orchestration. In order to gain those skills in automation/orchestration, it's pretty likely that you're going to end up having to work with APIs. Hence, why I'm talking about this stuff, and why I think this particular application can be helpful for non-programmers like me (and maybe you?) to build their skills. Hopefully this makes sense.

In any case, enough about that---let's start talking about Paw. Once you download it (there is a free trial available), you'll get a blank "document" window that looks something like this:

![Blank Paw document window](/public/img/blank-paw-window-small.png)

_(Click [here][link-2] for a full-size version of the image.)_

On the left-most side of the window are the various URL requests that are found in this Paw document (each Paw document is a collection of URL requests, each with its own parameters, headers, etc.). The center pane has the details for the selected URL request, and the right-most pane shows the response of the URL request (once you've sent it to the remote server).

Before we go any further, there's something you'll want to add to Paw that I think is pretty helpful: it's a code generator that shows you the equivalent HTTP request you're making in another language. I'll leave you to figure out the details (go to Paw &gt; Extensions &gt; Find Extensions), but go ahead and install the cURL Code Generator. This will generate an equivalent cURL command for what you create in Paw's graphical interface.

To provide some continuity with my other API-related posts, I'll use the OpenStack APIs as the example for this post. Let's start with getting an authentication token from Keystone:

1. In the URL section in the middle pane, change "GET" to "POST" and put in the URL for your Keystone service endpoint (it will be something like `http://192.168.100.100:5000/v2.0/tokens`).
2. Under the URL section, select "Body" and enter the JSON text that contains the username, password, and tenant. The JSON text would look something like this:

        { "auth": { "passwordCredentials": { "username": "demo",
        "password": "password" }, "tenantName": "demo" } }

3. Click "Headers" and add a header with the name "Content-Type" and the value "application/json". (These values should auto-populate.)
4. Assuming you have the cURL Code Generator installed, change "HTTP" to "cURL", and Paw will show you the cURL command line that corresponds to the request you just built.
5. Click the little arrow at the end of the URL bar, and---if everything worked correctly---the right-hand section of the window should show "200 OK" at the top and the response body underneath. Here's a screenshot:

![URL request and response](/public/img/paw-request-response-small.png)

_(Click [here][link-3] for a full-size version of the image.)_

Paw has actually taken the response from the server and parsed it into objects, but you can see the JSON text by changing "JSON" to "JSON Text", or even to "Text" to see the unformatted response (the "Raw" view also includes HTTP headers). This will give you an idea of the response from the server. If you're unfamiliar with JSON text, you may find viewing the parsed response easiest. Switch back to "JSON", and you can easily find the token ID, which you'll need for future URL requests.

Here's where Paw comes in really handy: double-click on the token ID value to select it, then right-click and select "Copy as Response Body Dynamic Value." This allows you to use this value in subsequent requests. Let's do that right now.

1. Click the plus symbol in the lower left corner of the window to create a new request.
2. Fill in the URL request (use "GET" instead of "POST" this time), supplying a URL to list tenants (something like `http://192.168.100.100:5000/v2.0/tenants`).
3. Click "Headers", and add a header named "X-Auth-Token". For the header value, right click and select Paste. Paw will put in something like "Response Parsed Body &lt;name&gt; access.token.id". This is a dynamic reference to the response value from the previous request.
4. Add another header, this time "Content-Type: application/json".
5. Click the little arrow at the end of the URL bar, and (assuming all is correct) the response area will show "200 OK". Here's a screenshot of what it might look like (click [here][link-4] for a full-size version of the image):

![Dynamic response](/public/img/paw-dynamic-response-small.png)

This should give you a rough idea of what you could do, but I'll carry it out a little bit:

* With the response from the list of tenants, you could copy the tenant ID as a response body dynamic value. You could then use this dynamic value in a subsequent request to list all the instances belonging to that tenant.
* In the response with the list of a tenant's instance, you could copy the instance ID from one of the instances as a response body dynamic value to retrieve all the information about a particular instance (in this case, you'd need to use both the tenant ID dynamic response value and the instance ID dynamic response value).

Pretty cool, eh? Along with the cURL Code Generator, you can see the `curl` commands used to make each request. Install the Python + Requests Code Generator, and you can see Python code that will generate the HTTP request. Install the HTTPie Code Generator, and you can see the command line for HTTPie that would generate this request. This is one of the key reasons I like Paw for someone like me who is just learning his way around various languages and command-line tools. Using the response parser, you can get a better familiarity with JSON objects and how to use tools like `jq` to filter the JSON response.

Anyway, I hope this has been helpful in some way. I know that I'll likely find Paw---and similar tools---very helpful as I continue on my full-stack journey.



[link-1]: https://luckymarmot.com/paw
[link-2]: /public/img/blank-paw-window.png
[link-3]: /public/img/paw-request-response.png
[link-4]: /public/img/paw-dynamic-response.png
[xref-1]: {{< relref "2014-02-19-using-curl-to-interact-with-a-restful-api.md" >}}
[xref-2]: {{< relref "2015-11-11-handy-cli-tool-json.md" >}}
