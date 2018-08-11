---
author: slowe
categories: Explanation
comments: true
date: 2018-07-19T17:00:00Z
tags:
- Linux
- CLI
- Git
- GitHub
- JSON
- Development
title: Cloning All Repositories in a GitHub Organization
url: /2018/07/19/cloning-all-repositories-in-a-github-organization/
---

I've recently started playing around with [Ballerina][link-3], and upon the suggestion of some folks on Twitter wanted to clone down some of the "official" Ballerina GitHub repositories to provide code examples and guides that would assist in my learning. Upon attempting to do so, however, I found myself needing to clone down 39 different repositories (all under a single organization), and so I asked on Twitter if there was an easy way to do this. Here's what I found.<!--more-->

Fairly quickly after I posted [my tweet asking about a solution][link-4], a follower responded indicating that I should be able to get the list of repositories via the GitHub API. He was, of course, correct:

    curl -s https://api.github.com/orgs/ballerina-guides/repos

This returns a list of the repositories in JSON format. Now, if you've been paying attention to my site, you know there's [a really handy way of parsing JSON data at the CLI][xref-1] (namely, the `jq` utility). However, to use `jq`, you need to know the overall _structure_ of the data. What if you don't know the structure?

No worries, [this post][xref-2] outlines another tool---`jid`---that allows us to interactively explore the data. So, I ran:

    curl -s https://api.github.com/orgs/ballerina-guides/repos | jid

This let me explore the data being returned by the GitHub API call, and I quickly determined that I needed the `clone_url` property for each repository. With this information in hand, I can now construct this command:

    curl -s https://api.github.com/orgs/ballerina-guides/repos |
    jq -r '.[].clone_url'

Now I have a list of all the clone URLs for all the repositories, right? Not quite---the GitHub API paginates results, so a minor adjustment is needed:

    curl -s https://api.github.com/orgs/ballerina-guides/repos?per_page=200 | jq -r '.[].clone_url'

From here it's a simple matter of piping the results to `xargs`, like this:

    curl -s https://api.github.com/orgs/ballerina-guides/repos?per_page=200 | jq -r '.[].clone_url' | xargs -n 1 git clone

Boom! Problem solved. As fate would have it, I'm not the only one thinking along these lines; here's [another example][link-1]. Several others also  suggested solutions involving Ruby; here's [one such example][link-2] (this is written for GitHub Enterprise but should work for "ordinary" GitHub).

Naturally, further tweaks to the API URL may be necessary; if you needed private repos, for example, then you'll have to add `&type=private` on the URL. Of course, that also means you'll need to supply authentication details...but you get the idea.

I hope others find this useful! (And thanks to those who took the time to respond on Twitter, I appreciate it!)

[link-1]: https://medium.com/@kevinsimper/how-to-clone-all-repositories-in-a-github-organization-8ccc6c4bd9df
[link-2]: https://gist.github.com/stevemart/06ace005f82691435d0b#clone-all-ghe-repos
[link-3]: https://ballerina.io/
[link-4]: https://twitter.com/scott_lowe/status/1020075780079083520
[xref-1]: {{< relref "2015-11-11-handy-cli-tool-json.md" >}}
[xref-2]: {{< relref "2018-06-28-more-handy-cli-tools-json.md" >}}
