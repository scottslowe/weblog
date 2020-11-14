---
author: slowe
categories: Explanation
comments: true
date: 2013-06-14T09:00:00Z
slug: reducing-the-friction-using-keyboard-shortcuts
tags:
- Automation
- macOS
- Productivity
title: 'Reducing the Friction: Using Keyboard Shortcuts'
url: /2013/06/14/reducing-the-friction-using-keyboard-shortcuts/
wordpress_id: 3215
---

One thing that I personally have found extremely helpful in making common tasks easier---a key aspect of the "reducing the friction" idea I've been discussing---is the use of keyboard shortcuts. In this post, I want to talk about how you might be able to maximize your use of keyboard shortcuts to help improve your personal efficiency.

In my view, there are three aspects to using keyboard shortcuts more extensively:

1. Using a keyboard-based launcher/search tool

2. Using a system-wide hotkey tool

3. Using (and/or customizing) application-specific keyboard shortcuts

Let's take a quick look at each of these.

## Keyboard-Based Launcher/Search Tool

I don't think it's any secret that I'm a fan of [Quicksilver](http://qsapp.com/), regarded by many to be the quintessential tool for OS X in this category. There are other tools in this category, of course; examples include [LaunchBar](http://www.obdev.at/launchbar) and [Alfred](http://www.alfredapp.com/). The key value that all these tools add to an existing system is the ability to quickly and easily search for something or launch an application without having to take your hands off the keyboard. Most of these tools also take an "object-oriented" approach that lets you use them to open URLs, open documents or folders, perform web searches, access browser bookmarks, and more.

## System-Wide Hotkey Tool

While tools such as Quicksilver (or LaunchBar or Alfred) have some of this functionality as well, I'm specifically thinking of a utility like [FastScripts](http://www.red-sweater.com/fastscripts/). FastScripts allow you to bind AppleScripts to keyboard shortcuts, either on a system-wide basis or on an application-specific basis. There are other tools similar to this, but this is one with which I am very familiar (I think [Keyboard Maestro](http://www.keyboardmaestro.com/main/) offers similar functionality, plus other features as well.)

Here are some potential ways you could use this tool to help reduce the friction:

* Perhaps you want to be able to compose a new e-mail message from any application on your system. You could write an AppleScript that launches Mail.app and composes a new message, then use FastScripts to create a system-wide shortcut key. Now, in whatever application you're in, you can quickly and easily bring up a window to send a new e-mail message---all without taking your hands off the keyboard or having to switch between applications.

* Maybe you want to make it even easier to launch a particular application. You could write a _very_ simple AppleScript that launches the application, then use FastScripts or its equivalent to create a system-wide hotkey to launch the app. (This sort of functionality is especially useful if you aren't using a tool like Quicksilver, LaunchBar, or Alfred.)

* Like with an application, an AppleScript to open a commonly-accessed folder is pretty simple. Think about being able to use a quick keyboard shortcut to access folders that you frequently use, without having to navigate through the folder structure or switch applications to get there. Handy!

## Application-Specific Keyboard Shortcuts

There are two parts to this particular aspect of using keyboard shortcuts:

1. Using something like FastScripts, but on an application-specific basis

2. Using OS X's System Preferences application to customize keyboard shortcuts more to your liking

Use case #1 I've kind of already described in the previous section, so I won't go into it again. There are _lots_ of potential use cases here---creating a keyboard shortcut for an AppleScript that archives the selected messages is one example that might help accelerate your e-mail processing workflow.

Use case #2 is a bit more interesting, I think. A little-known fact is that you can use System Preferences to create new keyboard shortcuts for specific features within applications **as well as** customize existing keyboard shortcuts. Here's an example. In some of my applications, the Export command (for saving a file in a different format) used Option-Command-E, but in other applications it didn't. Since keyboard shortcuts are a little like muscle memory, using the same keyboard shortcut across multiple applications makes it easier and simpler. So, I used System Preferences so that more of the applications I use regularly have the same keyboard shortcut (where possible) for the Export function.

I hope some of these ideas are useful, or that they spark new ideas you might have about how you might reduce the friction in your own workflows. I'd love to hear any ideas or suggestions that other people have, so please speak up in the comments below. Thanks!
