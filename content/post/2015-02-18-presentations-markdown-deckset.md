---
author: slowe
categories: Review
comments: true
date: 2015-02-18T09:55:00Z
tags:
- macOS
- Productivity
- Markdown
title: Presentations in Markdown Using Deckset
url: /2015/02/18/presentations-markdown-deckset/
---

Over the past couple of years, Markdown has become an important part of my computing landscape. I've transitioned almost all of my text-based content creation, including this blog, over to Markdown. I'd also been looking for ways that I might be able to extend my use of Markdown into creating presentations as well, but hadn't---until recently---found a tool that fit into my workflow. Then I started using [Deckset][link-1].

The idea behind Deckset is not unique; there are other projects out there that do the same sort of thing. ([Remarkjs][link-2] is one example that I've also used; more on that in a moment.) You create your presentation in Markdown, using headings, bulleted lists, numbered lists, etc. Markdown is just plain text, so you can use any plain text editing tool you like for this part. Deckset itself is OS X-specific, but the content remains platform- and application-independent (use any text editing tool on any platform you like).

Because Markdown isn't natively suited to creating presentations, Deckset---along with all the other solutions I tried---have to add some "extensions" to Markdown. For example, in Deckset's case:

* You'll use three hyphens (`---`) to denote the start of a new slide.
* You can use a `[fit]` option to have text (or an image) expand to fill available space.
* Standard Markdown syntax for images is extended with options to align the image left, center, or right.

As I mentioned, every tool I tried that provides this same sort of functionality (creating a slideshow from Markdown content) had to add Markdown extensions to handle slideshow-specific tasks. Compared to some of the other tools I tried, Deckset does a good job of minimizing these extensions and keeping the content fairly close to regular Markdown.

However, minimizing these Markdown extensions comes with a price, and that price is flexibility. Some of the other, similar tools that offer more complex Markdown extensions can offer greater flexibility in text placement, text sizing, and image manipulation through these custom extensions.

At the same time, greater flexibility comes with a price as well. Those other tools typically require a greater understanding of Cascading Style Sheets and/or HTML, and the content source starts to look less like Markdown and more like some type of fairly complex programming language. Remarkjs (whose source is [available on GitHub][link-3]) is another great tool that I used for a short while, and it offered great flexibility. However, the content source also ended up looking very complex.

This isn't a knock against either Deckset or Remarkjs; rather, just be aware of the trade-offs inherent in each solution. In the end, I selected Deckset over Remarkjs because I wanted to end up with Markdown content sources that were as close as reasonably possible to regular Markdown. In return, I had to give up some of the flexibility that Remarkjs offered. (If you're looking for a cross-platform tool, though, Remarkjs is a great solution.)

One of the other reasons I selected Deckset from among the various tools that I tried was PDF export. I publish presentations on SpeakerDeck and Slideshare after they've been given, and having the ability to _easily_ export to high-quality PDF was a big plus. (It's certainly possible to export to PDF with the other solutions, but with Deckset it's super easy.)

Of course, Deckset comes with standard functionality like presenter display, speaker notes, etc.

If I had to pick one problem with Deckset, it's the lack of ability to customize (or supply) your own themes. The application ships with 6 (or 7?) prebuilt themes, and each theme has different color variations. However, you can't edit the prebuilt themes, and you can't add your own themes. I'd love to see Unsigned Integer (the company behind Deckset) add the ability for the community to build themes for Deckset that can be shared within the Deckset community.

Aside from that one complaint, I've been pretty happy with Deckset. If you're an OS X user and are looking for a good Markdown-based presentation solution, I'd definitely recommend giving Deckset a try.

[link-1]: http://www.decksetapp.com/
[link-2]: http://remarkjs.com/
[link-3]: https://github.com/gnab/remark
