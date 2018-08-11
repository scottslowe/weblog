---
author: slowe
categories: Education
comments: true
date: 2015-01-26T12:01:00Z
tags:
- Git
- Collaboration
- CLI
- Development
- GitHub
title: Using Git with GitHub
url: /2015/01/26/using-git-with-github/
---

Building on [my earlier non-programmer's introduction to Git][xref-1], I wanted to talk a little bit about using Git with [GitHub][link-1], a very popular service for hosting Git repositories. This post, in conjunction with the earlier introductory post on Git, will serve as the basis for a future post that talks about how to use Git and GitHub to collaborate with others on an open source project hosted on GitHub.

If you aren't familiar with Git and haven't yet read the earlier introductory post, I _strongly_ recommend reading that post first.

Recall that Git is a distributed version control system (DVCS), and is designed to operate in such a way that full copies of repositories exist on multiple systems. This means you (as a single user) might have multiple copies of a repository across multiple systems. So how does one keep these repositories in sync? Generally, this would be handled via the use of a "server-side" repository to which the various repository clones are linked via a Git remote. This server-side repository might be hosted on an internal server or on a public server, and you may be connecting to it using the Git protocol, SSH, or HTTP(S). You would then push and/or pull changes to/from that server-side repository using various `git` commands (I'll discuss those in a moment). In this sort of arrangement, GitHub fills the role of the server-side repository---you'll push and pull changes to/from GitHub to clones of the repository on your laptop, your development workstation, and/or your server at work.

What you _won't_ do, though, is make changes directly to the GitHub repository. This threw me off in the early days of my journey to learn Git. In order to make changes to a repository hosted on GitHub, you _must_ have a local copy of the repository---in other words, you _must_ clone the repository to your local system. Only after you have a clone of the repository locally can you make changes. (**UPDATE:** A reader pointed out that GitHub's web site does offer  editing functionality, so _technically_ you can make changes directly to the GitHub repository.)

I talked about how to clone a repository in my earlier article, but let's look at it again. GitHub supports cloning via SSH as well as via HTTPS; I tend to use HTTPS as it is more likely to work in all environments (SSH can be blocked in many environments). So, to clone the GitHub repository for my weblog, I'd use the `git clone` command, like this:

	git clone https://github.com/scottslowe/lowescott.github.io.git

The generic form of this command is `git clone <URL>`. As I mentioned in my earlier Git article, this clones the entire repository down to your local system and automatically adds a Git remote (a link, if you will) back to the repository on GitHub. (You can see the Git remote by running `git remote -v`.) In the specific instance of using GitHub, you'll automatically have a Git remote named `origin` that refers to the GitHub repository.

Once you have the repository cloned to your system, you can begin to work with the repository: making changes, building your changeset with `git add` (to add new files to be tracked by Git or add changed files to the next commit), and committing changes to the repository's commit history with `git commit`. None of this changes when working with a repository cloned from GitHub.

What _does_ change, though, is that now you have the ability to take the changes you've made to your local repository and send them up to GitHub. You do that using the `git push` command. For example, if I wanted to push changes to the GitHub repository for my weblog, I'd use this command:

	git push origin master

The generic form of this command is `git push <remote> <branch>`. Recall that GitHub automatically added the `origin` remote when you cloned the repository, so that's why this command uses `origin`. The `master` comes from the default "master" branch. If I were working in a different branch (I haven't discussed branches yet, so bear with me), then the command would look a bit different. To ensure that you have permissions to push changes to the repository, GitHub would prompt you for credentials. This means that you will need a GitHub account, naturally. (Note that on some systems, the credentials can be saved and automatically supplied in the future. OS X is one such system.)

It can be difficult sometimes to keep track of whether you've pushed changes to GitHub or not, so the `git status` command helpfully tells you if your local repository is ahead of the GitHub repository (meaning changes have been committed locally but not pushed to GitHub).

If you have multiple clones of the repository (say, a clone on your laptop and a clone on a development workstation at the office), you can keep those clones in sync by pushing changes to GitHub (using `git push`) and then pulling those changes down on other systems. Naturally---as you've probably guessed already---the command to do that is `git pull`. If I pushed some changes to my weblog's repository from my MacBook Air and now need to update the clone of the repository on my Mac Pro, then I'd run this command:

	git pull origin master

Again, the generic form of the command is `git pull <remote> <branch>`, so if the Git remote to the GitHub repository was named something other than `origin` and/or you're working in a different branch than `master`, the command would look different. The `git pull` command pulls down all the changes from GitHub and then applies them to the local clone to bring it up to date.

To quickly recap, then:

* GitHub acts as a "server-side" Git repository to which changes (commits) can be pushed or pulled.
* You can't make changes directly to a GitHub repository; you have to clone the repository down to your local system using `git clone`.
* Cloning a repository from GitHub automatically adds a Git remote named `origin` that points back to the GitHub repository.
* Changes you've committed locally can be pushed to GitHub using the `git push` command. GitHub uses your GitHub account to determine if you are allowed to push changes to a particular repository.
* Changes you've pushed to GitHub can be pulled down to another clone of the repository using `git pull`.
* Multiple clones of the same GitHub repository are kept in sync through the combination of pushing changes to GitHub (using `git push`) and pulling those changes down using `git pull`. (Note: For this reason it's a good idea to get into the habit of using `git status` to check the current status of a repository before starting to make changes---you might need to run a `git pull` first to get the latest changes.)

## Additional Resources

I found [Matt Brender's #vBrownBag session on using Git and GitHub][link-2] to be useful. As I mentioned in the introductory Git post, I also found GitHub's online help pages to be useful, and I **really** encourage you to read the _Pro Git_ book by Scott Chacon. It's available for download [here][link-3].



[link-1]: https://github.com
[link-2]: http://professionalvmware.com/2014/10/vbrownbag-devops-follow-up-git-with-matthew-brender-mjbrender/
[link-3]: http://www.git-scm.com/book/en/v2
[xref-1]: {{< relref "2015-01-14-non-programmer-git-intro.md" >}}
