---
author: slowe
categories: Information
comments: true
date: 2015-04-06T15:00:00Z
tags:
- Productivity
- macOS
title: The Plain Text Productivity Experiment
url: /2015/04/06/plain-text-productivity-experiment/
---

I'm a big fan of plain text-based formats and tools, for a variety of reasons. As a result, I've moved almost all of my writing into plain text formats, primarily Markdown (MultiMarkdown, to be specific), and I recently spent a couple of months experimenting to see if a plain text-based productivity system would work for me as well. Here's what I found.

&lt;aside&gt;I won't go into a great amount of detail on _why_ I wanted to see if a plain text-based productivity system would work, as I've discussed the merits of using plain text-based formats before (platform portability, application portability, longevity, etc.).&lt;/aside&gt;

I first looked at Gina Trapani's [todo.txt system][link-1], which is plain text-based and has a thriving community built around its format. The todo.txt format is very straightforward and very simple---almost _too_ simple. After trying it for a short while I found that it just wasn't flexible enough to meet my needs.

A few weeks later, I stumbled on the TaskPaper format (named after [the Mac app of the same name][link-2]). Also plain text-based, the TaskPaper format also had a thriving community of applications and tools built around the format, and while also very simple the TaskPaper format was extremely flexible. For example, not only does the TaskPaper format support tags (like "@today" or "@telephone"), but the TaskPaper format supports values embedded in the tags. So, for example, using a tag like "@due(2014-07-15)" allows you to specify a value for the "@due" tag. Being able to embed values in the tags allows a lot of flexibility, such as being able to model start dates, due dates, and task dependencies---for example, using "@waiting(Bob)" to indicate a task is waiting on something from your co-worker Bob. TaskPaper also had tools like [a BBEdit language module][link-3] and [an OmniOutliner exporter][link-4], both very handy. The Mac app also sported extensive AppleScript support, which meant I could customize/extend its functionality over time.

While amazingly powerful for an app that works with plain text-based files, after a few months I found that TaskPaper wasn't cutting it for me. That's not to say that it won't work for you, only that it wasn't working _for me._ After a while of using TaskPaper (both the app and the plain text file format), these limitations/drawbacks became significant enough to really get my attention:

* _Handling future tasks is a problem._ One of the things I like to be able to do is capture a task I know I'm going to need to do at some point in the future. Then I know it's in my "trusted system," and I don't need to worry about it. All I need to do is assign a start date to the task---to denote when the task becomes available to do---and my trusted system will let me know when it's time to do the task. TaskPaper (the file format) allows you to embed a start date in a tag, like "@start(2016-01-01)", but TaskPaper (the application) doesn't know what to do with it. Before the start date, the task is still in your list of active tasks, and isn't marked or formatted differently to denote that it is a future task. This made it difficult---though not impossible---to effectively track future tasks.
* _Handling repeating tasks is a problem._ TaskPaper (both the format and the application) had no facility for repeating tasks. For repeating tasks, I would have to manually duplicate the existing task, edit the duplicate (if needed), then mark the first task complete. Yes, you could use a tag like "@repeat(weekly)" or similar, but the application doesn't do anything with such tags.
* _Handling tasks with due dates is a problem._ Although TaskPaper (the file format) supports dates inside a "@due" tag, TaskPaper (the application) won't do anything with the tags or the values in the tags. You have write and manually run a separate set of AppleScripts to check the dates in the tags against the current date to see if the task is due soon or even overdue.
* _Handling lots of tasks is a problem._ I like to capture all my tasks in my trusted system, and as a result my task list ends up being quite long. Although TaskPaper (the application) has an excellent search/query system with which to find tasks, managing a long list of tasks is still challenging. I tried breaking it up into separate TaskPaper files, but that only made the matter worse; now I had tasks split across multiple files with _no_ way of filtering/searching across them.
* _Keeping task lists synced across computers can be a problem._ TaskPaper (the application) doesn't have its own sync service; it relies on something like Dropbox. If you accidentally leave the application running with a file open on one computer, you can't modify that file on any other computer (the file is locked for changes). This made keeping task lists synced across computers challenging---if I didn't remember to kill TaskPaper on my Mac Pro at home before leaving on a trip, then I couldn't work with my task lists on my MacBook Air while I was traveling.

None of these problems are insurmountable, but the friction they cause---recalling that I use the term "friction" to denote how seamless or streamlined a process is, where reducing the friction (streamlining the process) is a good thing---made the trusted system feel...well, less trusted. So, after several months of trying to make a plain text productivity system work, I'm switching back to [OmniFocus][link-5]. While OmniFocus isn't perfect---no system is---it does address the challenges that I was experiencing with TaskPaper.

While plain text-based formats have worked wonderfully for me for writing and presentations, it looks like the existing raft of plain text-based productivity solutions don't have quite the functionality and robustness that I need. Others, though, might find that they work perfectly fine. So, don't be afraid to run your own plain text productivity experiment...you might be surprised at the results!



[link-1]: http://todotxt.com
[link-2]: http://www.hogbaysoftware.com/products/taskpaper
[link-3]: http://code.google.com/p/taskpaper-bbedit/
[link-4]: https://github.com/psidnell/oo2taskpaper
[link-5]: http://www.omnigroup.com/omnifocus/
