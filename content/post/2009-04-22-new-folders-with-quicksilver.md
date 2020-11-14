---
author: slowe
categories: Tutorial
comments: true
date: 2009-04-22T18:52:55Z
slug: new-folders-with-quicksilver
tags:
- macOS
title: New Folders with Quicksilver
url: /2009/04/22/new-folders-with-quicksilver/
wordpress_id: 1311
---

Prompted by [this Twitter status update](http://twitter.com/pokho/statuses/1584102830), I started looking around for a way to create a new folder using Quicksilver. For those of you that aren't familiar with Quicksilver, it's a bit hard to describe. I think someone once described it as an "extensible interface for manipulating objects" or some such. In any case, it is an _extremely_ powerful tool for streamlining many, many operations on your Mac.

So, in case you're interested, there are two ways to do this. Here's the first method:

1. Invoke Quicksilver. On my Mac, I use Opt+Space to invoke Quicksilver.

2. In the first pane, start typing to have Quicksilver try to find the object you're seeking. In this case, we're trying to select the parent folder for the new folder we are going to create. So, if we wanted to create a new folder in our personal Documents folder, we'd start by typing ~ (tilde). That would select our Home folder. Then start typing Documents until it matches that. Continue until you've found the parent folder of the new folder you are going to create.

3. Press Tab to move the action pane and select New Folder. (Just start typing New Folder until it matches.)

4. Press Tab to move to the object pane. You should be in a text entry field. Type in the name of the new folder you'd like to create. Press Enter when you're done.

5. Quicksilver creates the folder, and then places the new folder in the subject pane with a default action of Open. Press Enter and it will open the new folder you just created.

Pretty handy, eh?

Here's the second method. This method assumes you have a Finder window open and can select the parent folder of the new folder you want to create. For example, let's say that I have a folder in my personal Documents folder named Customers, and within that I want to create a folder named XYZCorp.

1. Use Finder to navigate to the parent folder of the new folder that you want to create. Click once to select it.

2. Invoke Quicksilver and press Cmd+G to take the object selected in Finder and put it in the subject pane.

3. Press Tab to move the action pane and select New Folder. (Just start typing New Folder until it matches.)

4. Press Tab to move to the object pane. You should be in a text entry field. Type in the name of the new folder you'd like to create. Press Enter when you're done.

5. Quicksilver creates the folder, and then places the new folder in the subject pane with a default action of Open. Press Enter and it will open the new folder you just created.

I think I like the first method better, but both approaches are pretty good.

Feel free to share other Quicksilver tips you may have in the comments. Thanks!
