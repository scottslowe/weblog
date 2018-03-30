---
author: slowe
categories: General
comments: true
date: 2016-05-28T00:00:00Z
tags:
- Podcast
title: Podcast Update
url: /2016/05/28/podcast-update/
---

As many of you probably know, I launched a new podcast, called the Full Stack Journey Podcast, back in January. (Here's [the blog post][xref-1] announcing the new podcast.) In this post, I wanted to provide a quick update on the podcast.

## Dedicated Website Now Up

First, the podcast now has [its own website!][link-1] Like this site, the Full Stack Journey site is a [Jekyll][link-2]-powered site hosted on GitHub (here's [the site's repository][link-3]). I find the Jekyll+GitHub Pages workflow works really well for me, so leveraging the same workflow for the Full Stack Journey site---as opposed to using WordPress or some other CMS---will (hopefully) help make it easier to continue to produce and publish the podcast.

## Late Episodes Available

The effort involved in getting the dedicated site up took up a fair amount of time over the last few weeks. This leads me to the second point, which is that I've [published episode #4 with Brent Salisbury][link-5], and will soon (in the next few days) be publishing episode #5 with Patrick Kelso. These episodes are _very_ late (sorry!). June's episode shouldn't be as late, and I'm aiming to be back on track with an early July episode (and then early in the month from there on). As with any new endeavor, it takes a bit of time to work out details and get used to how things fit in to your schedule.

## Podcast and Site Feeds

The Full Stack Journey podcast continues to be [available via iTunes][link-4] for easy subscription. I did have to move off SoundCloud, so if you're subscribed to the podcast via the SoundCloud feed you'll want to update the podcast feed URL:

    http://fullstackjourney.com/podcast.xml

I've already updated the iTunes podcast listing to reflect the new podcast feed, and I've configured SoundCloud to redirect subscribers to the new feed. Hopefully, this should make the transition as smooth as possible. I apologize for any inconvenience.

(For those that are interested, I moved from SoundCloud to hosting the podcast MP3's on [Amazon S3][link-6].)

So, to recap, here are the appropriate feeds for the podcast:

Web site RSS feed: [http://fullstackjourney.com/feed.xml][link-7]  
Podcast RSS feed: [http://fullstackjourney.com/podcast.xml][link-8]

If you have any questions or run into any issues, _please_ let me know.

## Seeking Guests

Finally, I'm seeking guests for the podcast. If you're someone who's pursued or is pursuing the goal of being a full stack engineer---that is, someone who is able to move across silos and among layers within the modern data center stack---I'd love to hear from you. Some key topics I'd like to explore in upcoming episodes include:

* AWS
* Azure
* Network automation
* Python scripting
* Kubernetes
* Storage in "stateless" environments
* Ansible

If you've listened the podcast before, you know I'm focused on making the podcast practical and useful. So, before you jump up and down and say, "Oooh, I know what Kubernetes is!", be aware that I'm going to want guests to be able to provide _practical_, _actionable_, _useful_ information to listeners to help move them along on their journey.

Of course, guests are not limited to only these topics; _any_ topic that would fit into the journey of broadening your technical horizons could potentially be a fit. Feel free to hit me up, either [via Twitter][link-9] or via e-mail (first name at fullstackjourney.com).

Thanks!



[link-1]: http://fullstackjourney.com
[link-2]: http://jekyllrb.com/
[link-3]: https://github.com/scottslowe/fullstackjourney
[link-4]: https://itunes.apple.com/us/podcast/full-stack-journey/id1073172158?mt=2
[link-5]: http://fullstackjourney.com/2016/04/14/full-stack-journey-ep004/
[link-6]: http://aws.amazon.com/s3/
[link-7]: http://fullstackjourney.com/feed.xml
[link-8]: http://fullstackjourney.com/podcast.xml
[link-9]: https://twitter.com/scott_lowe

[xref-1]: {{< relref "2016-01-07-launching-new-podcast.md" >}}
