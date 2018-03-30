---
author: slowe
categories: Education
comments: true
date: 2015-01-14T09:10:00Z
tags:
- CLI
- Linux
- Git
- Collaboration
- Development
title: A Non-Programmer's Introduction to Git
url: /2015/01/14/non-programmer-git-intro/
---

[Git][link-1] is a distributed version control system that is widely used by a number of open source projects. In this post, I'm going to provide a quick non-programmer's introduction to Git, and encourage readers to spend some time getting familiar with Git. I think it is a time investment that will pay off down the road.

First, I'm going to provide some definitions/brief explanations in order to establish a foundation upon which you can build your Git knowledge. A _version control system_ (sometimes just referred to as a VCS) is a system that tracks changes to files (or groups of files) over time.

The group of files that a VCS tracks is called a _repository_. The basic idea behind a VCS is that you could use it to "roll back" to an earlier version of any file (or group of files) in the repository in the event that the current version isn't working or isn't optimal. Almost all version control systems, including Git, support multiple repositories, and typically each repository would represent a particular project, component, or function. (I say "almost all version control systems" because there may be some VCS out there of which I am not aware that does not support multiple repositories.)

A _distributed version control system_ (sometimes called a DVCS) allows multiple systems to host entire copies of the repository, and allows the users on those systems to collaborate on changes to the repository. Git, as I mentioned in the opening paragraph, is a distributed version control system. Git's architecture as a DVCS is what drives many of the concepts around how to use (and collaborate with) Git.

Now that I've established some basics, let's get started with some specifics.

## Installing Git

You can download installers for Git from [the official Git website][link-1]. On Linux systems, you can also install Git using that distribution's package manager (`apt-get` on Debian/Ubuntu systems or `yum` on RedHat/CentOS systems). On OS X systems (recent releases, anyway), you can install Git by installing the "XCode command-line tools". As far as I am aware, Windows systems require the installer from the Git web site. If you want to follow along with some of the example commands I'll provide later in this post, go ahead and install Git on your system.

## Creating a Git Repository

Once you've installed Git, you're ready to create your first Git repository. Although Git allows you to collaborate with other users through copies of a repository, that's not required; you can use Git strictly as a local version control system if that works for you. (As I proceed with showing some examples on how to use Git, note that the command-line syntax for Git should be almost identical across systems. Even so, there may be slight variations from OS to OS. In this post, I'm using Git on OS X 10.9.5.)

With Git, repositories are tied to a directory. That is, you can turn a directory into a repository, but you can't turn a subset of files in a directory into a repository (the whole directory would become a repository).

To create a new repository, simply create the directory where you want the repository to be housed, then open a command prompt (or terminal window) and navigate to that directory. Once in the directory, use `git init` to initialize the directory as a Git repository. (Simple, right?)

You can also initialize an existing directory as a Git repository by simply running `git init` while in that directory. All existing files in the directory will show up as "untracked files" (more on that in a moment).

## Cloning a Git Repository

I've mentioned several times that Git, as a distributed version control system, is designed around letting multiple users work together. The basis of this collaboration lies in _cloning_ repositories. When you clone a Git repository, you get a full copy of the original repository plus a link (known as a "Git remote") back to the original.

Cloning a Git repository is super simple: just use the `git clone` command with a URL, like this (this is the GitHub repo that contains this blog):

	https://github.com/scottslowe/lowescott.github.io.git

I need to point out a couple of things here. First, don't conflate Git and [GitHub][link-2]. GitHub provides a service for hosting Git repositories, but you aren't **required** to use GitHub---you could use a private server running Git, or a competing public service like BitBucket. The underlying process for cloning a remote Git repository is the same. Second, this particular example uses HTTPS, but other protocols are supported. The protocol you should use will depend on how you're using Git. In this post, I'll use GitHub and HTTPS in my examples.

In addition to making a full copy of the remote repository on the system where you run `git clone`, a link back to the original remote is also created. You can see this link by running the command `git remote -v`, which will produce output something like this:

	origin	https://github.com/scottslowe/lowescott.github.io.git (fetch)
	origin	https://github.com/scottslowe/lowescott.github.io.git (push)

This link is needed in order to allow users to collaborate. Although each Git repository is completely standalone, this link allows users to push changes to a remote repository or pull changes from a remote repository.

There's so much more here, but in the interest of keeping the new things you're learning in this post manageable, I'll stop here for now. (More will come later, rest assured.)

## Making and Committing Changes

OK, so you have a Git repository. Now what? Well, now it's time to start adding files to the repository so you can track the changes to the files. You can copy new files into the repository, or create files from scratch---it's up to you. Once the files are there, though, you have to add them to be tracked by Git. Otherwise, they'll show up as "untracked" files.

To add files to the repository to be tracked, use `git add <path to file>`. You can use wildcards, so all these are valid commands:

	git add public/*.css
	git add headers/*.h
	git add _posts/2014-*.md

`git add` is a multi-purpose command---not only will you use it to add new files to a Git repository, you'll also use it to "add" already-tracked files that have been modified. That can be a bit confusing (it was to me, at least), so let me walk through it:

1. Let's say you cloned a repository, so you have a Git repository with a bunch of files already in it. All of these files have already been added to the repository (or else they wouldn't have been cloned to your system).
2. You add an entirely new file to the repository. In order for that file to be tracked by Git, you have to add it using `git add`.
3. You modify one of the existing files in the repository. In order for the changes to the file to be captured by Git, you must "add" the changed file using `git add`. I haven't talked about commits yet, but some Git references I've read explain that this use of `git add` is to add the changes to the next commit.

And speaking of commits...

Once you've used `git add` to add new/modified files, it's time to commit these changes (new files or changes to existing files) into the repository's history of changes. This is done using the `git commit` command. When you use `git commit`, you'll be prompted to enter what is called a commit message; the commit message is a brief description of the changes that are included in the commit. You can think of a commit as a "point-in-time" snapshot of the state of the repository.

Once you've committed your changes, then the cycle begins again: you add new files, you modify existing files, use `git add` to build the files and changes to a commit, then commit them to the repository's history with `git commit`.

At any point in the process, you can use `git status` to see the current status of the repository. The output of `git status` will tell you if there are untracked files (which should be added to the repository using `git add`), modified files (which should be added to the next commit using `git add`), or if the repository is "clean" (no untracked or modified files since the last commit).

## Practical Uses for Git

This is all well and good, but how can I (as a non-programmer) use a tool like Git? Here are a couple examples:

* You can use a Git repository to store the documentation for an IT project or service. The repository's commit history will reflect changes in the IT project or service over time.
* You can store configuration files in a Git repository. If a change to a configuration file produces adverse results, you can use the repository's commit history to go back to a previous version of the file. These configuration files could be application- or service-specific, or they could be configuration management files like Puppet manifests, Chef cookbooks, Ansible playbooks, or Salt states.
* Track revisions to automation scripts using a Git repository. This affords you the freedom to experiment with changes to the script, knowing that you can always roll back to a previous, working version of the script at any time.

These are just a few examples; I'm sure that as you experiment with Git you'll find plenty of other uses for it.

## Additional Resources

On my own journey of exploration/learning with Git, I found some of these resources to be quite helpful, so I'm including them here for you.

[Git Tutorials and Training - Atlassian][link-3]  
[GitHub Help - Git Bootcamp][link-4]  
[ProGit (open source Git book)][link-5] (I especially recommend this book!)  
[Git Reference][link-6]  
[Git Immersion][link-7]

Good luck!


[link-1]: http://git-scm.com
[link-2]: https://github.com
[link-3]: https://www.atlassian.com/zh/git/tutorial
[link-4]: https://help.github.com/categories/bootcamp/
[link-5]: http://git-scm.com/book/en/v2
[link-6]: http://gitref.org
[link-7]: http://gitimmersion.com
