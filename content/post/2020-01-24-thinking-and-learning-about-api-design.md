---
author: slowe
categories: Musing
comments: true
date: 2020-01-24T15:00:00-07:00
tags:
- Development
- Web
- JSON
title: Thinking and Learning About API Design
url: /2020/01/24/thinking-and-learning-about-api-design/
---

In July of 2018 I talked about [Polyglot][link-1], a very simple project I'd launched whose _only_ purpose was simply to [bolster my software development skills][xref-1]. Work on Polyglot has been sporadic at best, coming in fits and spurts, and thus far focused on building a model for the APIs that would be found in the project. Since I am not a software engineer by training (I have no formal training in software development), all of this is new to me, and I've found myself encountering lots of questions about API design along the way. In the interest of helping others who may be in a similar situation, I thought I'd share a bit here.<!--more-->

I initially approached the API in terms of how I would encode (serialize?) data on the wire using JSON (I'd decided on using a RESTful API with JSON over HTTP). Starting with how I anticipated storing the data in the back-end database, I created a representation of how a customer's information would be encoded (serialized) in JSON:

``` json
{
    "customers": [
        {
            "customerID": "5678",
            "streetAddress": "123 Main Street",
            "unitNumber": "Suite 123",
            "city": "Anywhere",
            "state": "CO",
            "postalCode": "80108",
            "telephone": "3035551212",
            "primaryContactFirstName": "Scott",
            "primaryContactLastName": "Lowe"
        }
    ]
}
```

It's pretty obvious, though, that this representation is closely tied to how I anticipate I'll store customer data in the back-end database, right down to the field names. One of the books I've been reading is _Building Microservices_, an O'Reilly book by Sam Newman (see [here][link-2]; I'll post a review of the book once I complete it). One of the things Newman cautions readers against in his book is exposing implementation details internal to a service to other services---did this qualify? Newman also discussed hypermedia as the engine of application state (HATEOAS, what an acronym!), but I'll be honest in admitting that it didn't click with me at first.

While still working through Newman's book, I added _REST API Design Rulebook_ by Mark Masse to the mix (see [my review][xref-2] of the book). Masse talked extensively about HATEOAS, but---as you can see in my review of the book---it still didn't click for me.

Next up, I added _RESTful Web APIs_ by Leonard Richardson, Sam Ruby, and Mike Amundsen to my reading list (here's [the O'Reilly page][link-3] for the book; I'm still reading the book so no review yet). Whether by repetition or by seeing the information presented in a different context, the discussion of HATEOAS and hypermedia by Richardson et al. finally made sense. It also unlocked some of the ideas presented by Masse and Newman earlier, so that I was able to really understand that I'd made several critical mistakes in my approach thus far:

* I chose a media type (JSON) before fully understanding the particulars of my application without realizing the constraints that media type creates.
* The client representation was too closely tied to the back-end/server-side implementation, making things far too brittle (what would happen if I needed to change the database schema?).
* I invented new field names instead of re-using properties and fields that are already well-known and well-accepted (like re-using information from an appropriate microformat).
* The client representation doesn't provide any information on state transitions (How is a customer record updated? How is a customer record deleted?) or relationships between resources (What's the relationship between an order record and a customer record? Or vice versa?).

It's clear that something _more_ than just encoding data in JSON is required.

The question, for me, is how much more? And what to include, exactly? Richardson et al. seem to advocate [the Collection+JSON format][link-4], but it's only a personal standard (see his book for definitions of various standards) and not necessarily widely accepted/used. However, even within Collection+JSON, the specific links provided as hypermedia are left as "implementation details." So what sort of information makes sense? 

Given that Polyglot is intended to be a simple order entry/lookup system, a few potential ideas come to mind:

* All discussions of HATEOAS and hypermedia agree that a "self" link is needed that provides the URL to the resource (customer record or order record) being returned.
* It seems reasonable to return "related" information. So, if viewing an order record, provide a link to the associated customer record (for example), the previous order, the next order, etc.

Clearly I have lots of work to do!

Now comes the task of shaping the limited work I've done so far into something more usable. I also need to finish reading _RESTful Web APIs_; although the content was difficult for me to grok at first, recently I've found that I'm beginning to more fully understand the concepts the authors are presenting. I still have lots of questions---how do things like OpenAPI fit in here?---so if you have answers, feel free to [hit me on Twitter][link-5]. This is definitely a learning exercise for me, so I welcome any other resources I should be using to help guide my way.

[link-1]: https://github.com/scottslowe/polyglot
[link-2]: http://shop.oreilly.com/product/0636920033158.do
[link-3]: http://shop.oreilly.com/product/0636920028468.do
[link-4]: http://amundsen.com/media-types/collection/format/
[link-5]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2018-07-23-bolstering-my-software-development-skills.md" >}}
[xref-2]: {{< relref "2018-09-04-book-review-rest-api-design-rulebook.md" >}}
