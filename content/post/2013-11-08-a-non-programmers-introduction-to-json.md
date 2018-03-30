---
author: slowe
categories: Education
comments: true
date: 2013-11-08T09:11:50Z
slug: a-non-programmers-introduction-to-json
tags:
- JSON
- Web
title: A Non-Programmer's Introduction to JSON
url: /2013/11/08/a-non-programmers-introduction-to-json/
wordpress_id: 3326
---

As part of some work I've been doing to stretch myself and my boundaries, I've recently started diving a bit deeper into working with REST APIs. As I started this exploration, one thing that kept coming up again and again was JSON. In this post, I'm going to _try_ to provide an introduction to JSON for non-programmers (like me).

Let's start with the acronym: "JSON" stands for "JavaScript Object Notation". It's a lightweight, text-based format, and is frequently used in conjunction with REST APIs and web-based services. You can find more details on the specifics of the JSON format at [the JSON web site](http://json.org/).

The basic structures of JSON are:

* A set of name/value pairs

* An ordered list of values

Now, that sounds simple enough, but let's look at some examples to really bring this home. The examples that I'll use are taken from API responses in my virtualized NVP/NSX lab using the NVP/NSX API.

First, here's an example of a set of name/value pairs (I've taken the liberty of making the raw output from the API calls more readable for clarity's sake; raw JSON data typically wouldn't have line returns or whitespace):

``` json
{
    "result_count": 3, 
    "results": [
        {
            "_href": "/ws.v1/lswitch/3ca2d5ef-6a0f-4392-9ec1-a6645234bc55", 
            "_schema": "/ws.v1/schema/LogicalSwitchConfig", 
            "type": "LogicalSwitchConfig"
        }, 
        {
            "_href": "/ws.v1/lswitch/81f51868-2142-48a8-93ff-ef612249e025", 
            "_schema": "/ws.v1/schema/LogicalSwitchConfig", 
            "type": "LogicalSwitchConfig"
        }, 
        {
            "_href": "/ws.v1/lswitch/9fed3467-dd74-421b-ab30-7bc9bfae6248", 
            "_schema": "/ws.v1/schema/LogicalSwitchConfig", 
            "type": "LogicalSwitchConfig"
        }
    ]
}
```

(Click [here](https://gist.github.com/scottslowe/7370142) for an option to download the code snippet above.)

Let's break that down a bit:

* Each object is surrounded by curly braces (referred to just as braces by some). The entire JSON response is itself an object---at least this is how I view it---so it is surrounded by braces. It contains three objects, which are part of the "results" array (more on that in just a second).

* Each object may have multiple name/value pairs separated by a comma. Name/value pairs may represent a single value (as with "result_count") or multiple values in an array (as with "results"). So, in this example, there are two name/value pairs: one named "result_count" and one named "results". Note the use of the colon separating the name from the associated value(s).

* The second item (object, if you will) in the API response is named "results", but note that its value isn't a single value; rather, it's an array of values. Arrays are surrounded by brackets, and each element/item in the array is separated by a comma. In this particular case---and this will vary from API to API, as far as I know---note that the "result_count" value tells you exactly how many items are in the "results" array, making it incredibly easy to iterate through the items in the array.

* In the "results" array, there are three items (or objects). Each of these items---each surrounded by braces---has three name/value pairs, separated by commas, with a colon separating the name from the value.

As you can see, JSON has a very simple structure and format, once you're able to break it down.

There are numerous other examples and breakdowns of JSON around the web; here are a few that I found helpful in my education (which is still ongoing):

[JSON Basics: What You Need to Know](http://www.elated.com/articles/json-basics/)  

[JSON: What It Is, How It Works, & How to Use It](http://www.copterlabs.com/blog/json-what-it-is-how-it-works-how-to-use-it/) _(This one gets a bit deep for non-programmers, but you might find it helpful nevertheless.)_  

[JSON Tutorial](http://www.w3resource.com/JSON/introduction.php)

You may also see the term "JSON-serialized"; this generally refers to data that has been formatted as JSON. To JSON-serialize data means to put it into JSON format; to deserialize JSON data means to parse (or deconstruct) the JSON output into some other format.

I'm sure there's a great deal more that could (and perhaps should) be said about JSON, but I did say this would be a non-programmer's introduction to JSON. If you have any questions, thoughts, suggestions, or clarifications, please feel free to speak up in the comments below.

**UPDATE:** I've edited the text above based on some feedback in the comments. Thanks for your feedback; the post is better for it!
