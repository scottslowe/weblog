---
author: slowe
categories: Explanation
comments: true
date: 2010-09-13T10:00:00Z
slug: reverting-project-behavior-in-omnifocus
tags:
- macOS
- Productivity
title: Reverting Project Behavior in OmniFocus
url: /2010/09/13/reverting-project-behavior-in-omnifocus/
wordpress_id: 2103
---

I recently updated [OmniFocus](http://www.omnigroup.com/products/omnifocus/) on my MacBook Pro to version 1.8, the latest version, and noticed something that was---to me, at least---quite disturbing. Projects that had no child actions were now displaying in my Contexts view! Besides running counter-intuitive to my (albeit limited) understanding of Getting Things Done (GTD), this new behavior just didn't make sense to me.

A quick Google search turned up [a forum discussion on the matter](http://forums.omnigroup.com/showthread.php?t=15345&page=1). Feelings were apparently running very strong about this matter, as individuals squared off on each side. Some people felt that having projects with no child actions appear in the Contexts view was perfectly natural. Their explanation was that completing child actions brought you ever closer to completing the project, and thus when all the child actions were complete you could then "complete" the project "action". Others, like myself, felt that projects shouldn't be included because you don't "do" a project, and putting a project in the list of things you "do" didn't make sense. After all, projects aren't actions and actions aren't projects.

Fortunately, there appears to be a fix. A hidden preference setting can be changed to prevent childless projects from appearing in Contexts view. The change is implemented as a URL, like this:

[omnifocus:///change-setting?ContextModeShowsParents=false](omnifocus:///change-setting?ContextModeShowsParents=false)

This URL would then change it back:

[omnifocus:///change-setting?ContextModeShowsParents=true](omnifocus:///change-setting?ContextModeShowsParents=true)

I searched for a way to change this setting using the more traditional `defaults write` technique, but couldn't find any settings in the OmniFocus preferences file that changed when I issued these commands. I tried converting them to XML format using `plutil` and comparing them with `diff`, but that didn't show any differences either. Searching the files using `grep` didn't reveal any potential candidates either, so it appears that the _only_ way to change this setting is to use the URL above. That implies to me that this setting is embedded inside the OmniFocus database, not in the preferences for the application.

In any case, if you're using OmniFocus 1.8 and don't want projects to show up in your Contexts view, you can change the behavior using the URL above.

**UPDATE:** It turns out that OmniFocus does have a menu command, on the View menu, to toggle this behavior. The Show/Hide Parent Items in Context View is the command you need. Thanks for all who pointed that out to me!
