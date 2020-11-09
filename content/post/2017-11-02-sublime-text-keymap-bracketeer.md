---
author: slowe
categories: Information
comments: true
date: 2017-11-02T21:00:00Z
tags:
- macOS
- Writing
- Productivity
title: A Sublime Text Keymap for Bracketeer
url: /2017/11/02/sublime-text-keymap-bracketeer/
---

I've made no secret of the fact that I'm a fan of [Sublime Text][link-1] (ST). I've evaluated other editors, like [Atom][link-2], but still find that ST offers the right blend of performance, functionality, customizability, and cross-platform support. One nice thing about ST (other editors have this too) is the ability to extend it via packages. [Bracketeer][link-3] is one of many packages that can be used to customize ST's behavior; in this post, I'd like to share a keymap I'm using with Bracketeer that I've found very helpful.<!--more--> 

Bracketeer is a package that modifies ST's default bracketing behavior. I first started using Bracketeer to help with writing Markdown documents, as it makes adding brackets (or parentheses) around existing text easier (it automatically advances the insertion point after the closing bracket). After using Bracketeer for a little while, I realized I could extend the keymap for Bracketeer to have it also help me with "wrapping" text in backticks and a few other characters. I did this by adding this line to the default keymap:

``` json
{
  "keys": [ "`" ],
  "command": "bracketeer",
  "args": {
    "braces": "``",
    "pressed": "`"
  }
}
```

With this line in the keymap, I could select some text, press the backtick character, and ST would surround the selected text in backticks and position the insertion point after the closing backtick. Handy! Without Bracketeer, doing this would have just replaced the selected text with a single backtick.

Unfortunately, there was a drawback: If I wanted just a single backtick, I'd still get a pair of them. In digging around for a solution/workaround, I found that ST's keymap allows you to specify a "context," or a condition, that applies to whether the keymap is applied.

To put this into action, I added a context to the keymap entry above to make it look like this:

``` json
{
  "keys": ["`"],
  "command": "bracketeer",
  "args": { "braces": "``" },
  "context": [
    {
      "key": "selection_empty",
      "operator": "equal",
      "operand": false,
      "match_all": true
    }
  ]
}
```

Configured this way, Bracketeer only activates _when text is selected._ When text isn't selected, then Bracketeer doesn't modify the default behavior. This allows me to use Bracketeer to easily surround selected text with just about any character: backticks, asterisks, pipe symbols, or underscores.

For a full copy of my modified Bracketeer keymap, see [this gist][link-4].

[link-1]: http://www.sublimetext.com/
[link-2]: https://atom.io/
[link-3]: https://packagecontrol.io/packages/Bracketeer
[link-4]: https://gist.github.com/scottslowe/108192e1a0d27028df73a1b2fc4a740a
