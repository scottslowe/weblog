---
author: slowe
categories: Explanation
comments: true
date: 2017-01-19T15:00:00Z
tags:
- Linux
- CLI
- Productivity
- Ubuntu
- macOS
title: Plain Text Productivity Redux
url: /2017/01/19/plain-text-productivity-redux/
---

Almost 2 years ago, I set out on [an experiment in plain text productivity][xref-1]. I won't say the experiment was a failure; I _did_ learn from the experiment, and gaining knowledge is usually a positive outcome. In the end, I switched back to [OmniFocus][link-1], the macOS- and iOS-specific app I'd been using previously. In the last few weeks, though, I've revisited the idea of a plain text productivity system as part of my migration to [Ubuntu Linux][link-3] as my primary desktop OS, and I think I've resolved some of the issues that were present in my last attempt.

To recap, in my previous attempt I settled on the TaskPaper format (named after [the macOS app of the same name][link-2]). The format is extraordinarily flexible, and the macOS app is more powerful than you might expect. However, I uncovered some issues that made the solution untenable; namely:

1. Handling future tasks is a problem.
2. Handling repeating tasks is a problem.
3. Handling tasks with due dates is a problem.
4. Handling lots of tasks is a problem.
5. Keeping task lists synced across computers can be a problem.

At the time, the app had no way to dynamically respond to information in tasks, such as due dates. If you set a due date for a task and that date passed, the app didn't (couldn't) notify you that you'd missed a due date. Similarly, the app couldn't let you know that a task was due soon. Frankly, these "limitations" are fully understandable given the plain text nature of the TaskPaper format, and not a knock against the application itself. (By the way, it's my understanding a new major release of the Mac app addresses some of these issues. I haven't tested it, so I can't say for certain.)

Due to the issues listed above, I moved back to OmniFocus. Since that time, though, I adopted [Sublime Text][link-4] as my primary text editor. Now, this might seem like an unrelated piece of information, but stay tuned---I'll show in a moment why this is important. Further, I very recently [decided to make Ubuntu Linux 16.04 my primary desktop OS][xref-2], and as a result I needed to revisit the applications I use. Given that OmniFocus is a Mac-only app, this meant a new task management/productivity solution was needed.

Sadly, the state of task management/productivity on Linux is pretty poor. The landscape is littered with the abandoned carcasses of task management apps that sprung up and were later discarded and left to die. Fortunately---and this is where Sublime Text comes into the picture---I found a package for Sublime Text that supports the TaskPaper format. (Here's [a link to the package][link-5], which you can install via Package Control.) The important thing to note is that not only does this package provide syntax support for the TaskPaper syntax, but it _also_ does other useful things:

* It places a gutter icon near tasks that are due soon or past due.
* It can apply different color/highlighting for tasks that are due soon or past due, providing additional visual differentiation.
* It can show time remaining until a task becomes due.
* It can filter out tasks based on "@tags" assigned to the task.
* It can "fold" to show only due/overdue tasks.
* It can preview your task list in HTML, or export it to HTML.

In short, this package addresses a number of the concerns with the original Mac app. Further, since Sublime Text is a cross-platform app (supported on Windows, Linux, and macOS), this means I can get the same functionality on all platforms. Based on my use over the last few weeks, I feel like using the PlainTasks package does a pretty good job of addressing issues #1 and #3 from my list above.

Issues #2 and #4 remain outstanding, but the biggest hurdle was issue #5---keeping my task lists synchronized across multiple systems. I think I've found a solution here, and testing over the last few weeks has not uncovered any major issues thus far. The solution is to keep separate files for each device: one for my Mac Pro workstation, one for my MacBook Air (which is soon to be retired), and one for my MacBook Pro running Ubuntu (which is soon to be replaced by a new corporate laptop running Ubuntu). I'll store these on [Dropbox][link-6], which means every device has a copy of every other device's tasks. Then, because these are just plain text files, I'll use file comparison/merge tools---tools that are well-understood given their extensive use within version control systems like Git---to merge changes between files. After all, merging plain text files that contain task lists is no different than merging plain text files that contain code. Why reinvent the wheel?

On Ubuntu, I can use [Meld][link-7] for this process; on macOS, I can use [Kaleidoscope][link-8]. Yes, it will take a couple of passes to converge, but the process is super-quick and quite easy. I haven't found a comparison/merge tool for iOS or Android yet, but I do have an iOS text editor (Editorial) that supports the TaskPaper format (as well as Markdown). Given that it's very likely I'd only need to use a mobile device when my primary laptop would be offline, I think I can get away with just editing the primary laptop text file from my mobile device. I haven't tested that part yet, so some additional fine-tuning may be necessary.

Using this process should largely address issue #5, leaving only #2 and #4. I intend to tackle issue #2 with calendar entries, though---to be honest---the state of calendaring on Linux is also pretty bad. I'm not sure about #4 yet.

So there you have it---a system for using plain text-based files for task management and productivity. Stay tuned for more updates as I continue to test and refine my process and tools.



[link-1]: http://www.omnigroup.com/omnifocus/
[link-2]: https://www.taskpaper.com/
[link-3]: https://www.ubuntu.com/
[link-4]: http://www.sublimetext.com/
[link-5]: https://packagecontrol.io/packages/PlainTasks
[link-6]: https://www.dropbox.com/
[link-7]: http://meldmerge.org/
[link-8]: http://www.kaleidoscopeapp.com/
[xref-1]: {{< relref "2015-04-06-plain-text-productivity-experiment.md" >}}
[xref-2]: {{< relref "2016-12-16-linux-migration-initial-progress-report.md" >}}
