---
author: slowe
categories: Tutorial
comments: true
date: 2015-09-04T13:40:00Z
tags:
- CLI
- Git
- GitHub
- Development
title: Checking Out GitHub Pull Requests Locally
url: /2015/09/04/checking-out-github-pull-requests-locally/
---

In this post, I'm going to show you how to use the Git command-line to check out [GitHub][link-2] pull requests locally. I take absolutely no credit for this trick! I picked this up from [this GitHub Gist][link-1], and merely wanted to share it here so that others would benefit.

The GitHub gist shows you how to modify the Git configuration for a particular repository so that when you run `git fetch` it will fetch all the pull requests for that repository as well. This is handy, but what I personally found _most_ helpful was a comment that showed the command to fetch a specific pull request. The command looks like this:

    git fetch origin pull/1234/head:pr-1234

Let me break that command down a bit:

- The `origin` in this case refers to the Git remote for this repository on GitHub. If you are using [the fork-and-pull method of collaborating via Git and GitHub][xref-1], then you will have multiple Git remotes---and the remote you want probably _isn't_ `origin`. For example, if you want to fetch a pull request from the original (not forked) repository, you'd want to use the name that corresponds to the Git remote for the original repository (I use `upstream` in these cases).
- The `pull/1234/head` portion refers to the pull request on GitHub. Every pull request has a number assigned. If the pull request is #10, then the path would be `pull/10/head`. If the pull request is #9182, then the path is `pull/9182/head`.
- The `:pr-1234` portion of the command points to the local branch into which you'd like the specified pull request to be fetched. **This branch should not already exist.** If you try to fetch into an existing branch, the command will fail, so specify the name of a branch you'd like created as part of the process.

Once you run the command and the fetch is successful, then you can switch to the branch with `git checkout pr-1234` (or whatever name you gave the new branch) and start reviewing the contents of the pull request. Very handy!

[link-1]: https://gist.github.com/piscisaureus/3342247
[link-2]: https://www.github.com/
[xref-1]: {{< relref "2015-01-27-using-fork-branch-git-workflow.md" >}}
