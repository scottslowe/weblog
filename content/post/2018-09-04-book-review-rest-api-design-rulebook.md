---
author: slowe
categories: Review
comments: true
date: 2018-09-04T12:00:00Z
tags:
- Development
- Web
title: 'Book Review: REST API Design Rulebook'
url: /2018/09/04/book-review-rest-api-design-rulebook/
---

_REST API Design Rulebook_ (written by Mark Masse and published by O'Reilly Media; more details [here][link-2]) is an older book, published in late 2011. However, having never attempted to design a REST API before, I found lots of useful information inside that really helped shape my understanding of REST APIs and REST API design.<!--more-->

(In case you're wondering why I was reading a book about REST API design, this ties into [my 2018 project list][xref-1] and [the software development project][xref-2] I recently launched.)

Overall, I found the book quite helpful and useful. If I had one complaint about the book, it would be the book's repeated insistence on referring to [WRML (Web Resource Modeling Language)][link-1], which---as I understand it---is a _proposed_ solution by the book's author to some of the challenges around REST API design. I get that the author is sold on the value of WRML, but at times the book felt very much like a WRML commercial.

Aside from that one complaint, the book's organization into a set of "rules" helped make the material reasonably consumable, and I appreciated the review of key terms at the end of each chapter.

I do still have some questions about REST APIs and REST API design; I suppose that's natural and expected for a newcomer like myself. In particular, the emphasis placed on HATEOAS (Hypermedia As The Engine of Application State) is still a bit unclear to me. Some things make sense---like including links to actions (provided by the API) that can be performed on an object---but there were times in the book when it seemed like HATEOAS was being overemphasized. However, I freely admit my perception could be due to my lack of practical experience in this area.

If you're looking to learn more about REST API design, there are most certainly worse places to look for some useful and practical information.

_Disclaimer: I am an O'Reilly author, but I did not receive any compensation from O'Reilly, any other publisher or reviewer, or the author of the book mentioned in this review. These thoughts are mine alone._

[link-1]: https://github.com/wrml/wrml
[link-2]: http://shop.oreilly.com/product/0636920021575.do
[xref-1]: {{< relref "2018-03-05-looking-ahead-2018-projects.md" >}}
[xref-2]: {{< relref "2018-07-23-bolstering-my-software-development-skills.md" >}}
