---
author: slowe
categories: Explanation
comments: true
date: 2020-03-08T17:00:00-07:00
tags:
- macOS
- JSON
- Blogging
title: Modifying Visual Studio Code's Bracketing Behavior
url: /2020/03/08/modifying-visual-studio-code-bracketing-behavior/
---

There are two things I've missed since I switched from [Sublime Text][link-3] to [Visual Studio Code][link-2] (I switched in 2018). First, the _speed._ Sublime Text is _so_ much faster than Visual Studio Code; it's insane. But, the team behind Visual Studio Code is working hard to improve performance, so I've mostly resigned myself to it. The second thing, though, was the behavior of wrapping selected text in brackets (or parentheses, curly braces, quotes, etc.). That part has annoyed me for two years, until this past weekend I'd finally had enough. Here's how I modified Visual Studio Code's bracketing behaviors.<!--more-->

Before I get into the changes, allow me to explain what I mean here. With Sublime Text, I used a package (similar to an extension in the VS Code world) called [Bracketeer][link-1], and one of the things it allowed me to do was highlight a section of text, wrap it in brackets/braces/parentheses, and then---this part is the missing bit---it would _deselect the selected text and place my insertion point outside the closing character_. Now, this doesn't sound like a big deal, but let me assure you that it is. I didn't have to use any extra keystrokes to keep on typing, I didn't need to deselect the text manually, I didn't need to manually move my insertion point outside the closing bracket or brace or whatever. It was, in a word, glorious.

Visual Studio Code does not do this. There is no equivalent to Bracketeer that I've been able to find. There is no documented setting I've been able to locate that would emulate this behavior. Instead, if you select some text and wrap it in brackets or braces or whatever, the text _remains selected_. You have to double-tap the right arrow to deselect the text and move out of the brackets, or get the mouse involved. No good.

Fortunately, I finally found a workaround. It might be a hacky workaround, but it's a workaround nevertheless. There are two pieces: a snippet, and a keybinding.

First, the snippet. You'll need a snippet for each character (bracket, curly braces, parenethesis, single quote, double quote, backtick) whose "wrapping" behavior you want to modify. The snippet looks like this:

```json
{
	"Brackets Wrap": {
		"prefix": "brackwrap",
		"body": [
			"[$TM_SELECTED_TEXT]$0"
		],
		"description": "Wrap selected text in brackets and position cursor outside the closing bracket"
	}
}
```

The magic here comes from `$TM_SELECTED_TEXT` and `$0`. The `$TM_SELECTED_TEXT` variable is replaced with the contents of the currently selected text in the editor. Since this snippet is for wrapping text in square brackets, you can see I've surrounded the text with square brackets. The `$0` indicates the final position of the insertion point after the snippet has been inserted. In this case, the final position of the insertion point is just after the closing bracket. So, what will happen when I invoke this snippet is this:

1. Code will place an opening square bracket before the selected text, and a closing square bracket after the selected text.
2. Code will place the insertion point after the closing bracket, deselecting the selected text in the process.

Bam! Almost exactly what I was seeking. To make it just a "natural" part of how Code operates is where the keybinding comes in:

```json
[
    {
        "key": "[",
        "command": "editor.action.insertSnippet",
        "when": "editorHasSelection && editorLangId == markdown",
        "args": {
            "langId": "markdown",
            "name": "Brackets Wrap"
        }
    },
]
```

The keybinding re-maps the keyboard so that when I press the `[` character when text is selected (note the `when` clause in there), it will insert the snippet I just showed you---in other words, when I press the `[` character when there's selected text, it will wrap the selected text in square brackets and then place my insertion point after the closing bracket. Bingo!

Finally, note the compound `when` clause, which ensures that this only fires when text is selected in a Markdown document. This keeps the keybinding from affecting other types of documents, where this behavior may not be desired. Oh, and here's a bonus---it works with multiple selections, too.

To emulate this same behavior for curly braces, single quotes, double quotes, or backticks, you'll need to define a snippet and a keybinding for each of them. Just change the character used to surround the text in the snippet, and then define a keybinding that re-maps that particular character on the keyboard.

There you have it! I know this seems silly in the grander scheme of things, but it has made my Visual Studio Code experience so much more pleasant. If you have questions, or if you have suggestions for improvement (maybe I missed something?), please feel free to [hit me up on Twitter][link-4]. Thanks for reading!

[link-1]: https://packagecontrol.io/packages/Bracketeer
[link-2]: https://code.visualstudio.com/
[link-3]: https://www.sublimetext.com/
[link-4]: https://twitter.com/scott_lowe
