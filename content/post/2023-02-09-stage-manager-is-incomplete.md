---
author: slowe
categories: Rant
comments: true
date: 2023-02-09T10:00:00-05:00
tags:
- macOS
title: Stage Manager is Incomplete
url: /2023/02/09/stage-manager-is-incomplete/
---

I've been using macOS Stage Manager off and on for a little while now. In Stage Manager, I can see the beginnings of what might be a very useful paradigm for desktop computing. Unfortunately, in its current incarnation, I believe Stage Manager is _incomplete_.<!--more-->

Note that I haven't yet tried Stage Manager on my iPad; my comments here apply _only_ to the macOS implementation.

For those of you who haven't yet tried Stage Manager yet, here's a screenshot of my desktop, taken while I was writing this blog post:

![Desktop screenshot of macOS with Stage Manager enabled](/public/img/desktop-with-stage-manager.png)

I'll draw your attention to the list of "recently used applications" on the left side of the screen. That's the "Cast" (a term used by Howard Oakley in [his great introductory article on Stage Manager][link-1]). As you can see in this screenshot, the Cast supports application groups---like having Slack and Mail grouped together---as well as single applications. This allows you to easily switch between groups of applications simply by clicking on the preview in the Cast (which, using Howard's terminology, moves the application or applications to the Stage).

This is the glimmer of a useful paradigm that I see in Stage Manager: being able to assemble groups of applications that you often use together and being able to easily switch between those groups of applications.

Unfortunately, the Cast of recent applications shown on the left of the screen is as far as Apple went with that paradigm, and this is why I say that Stage Manager is incomplete. When I use Stage Manager, I'm left asking questions like:

* **Why not extend Cmd-Tab to switch between application groups?** The Cmd-Tab application switcher isn't what I would call "Stage Manager" aware, so it just displays individual applications (not the Stage Manager application groups you've created). Yes, it still switches the applications between Cast and Stage, but why not display the application groups in the switcher?
* **Why not extend the Dock to show application groups?** Again, I feel like Apple needs to move the focus away from individual applications to application groups (which may be a single application). Why not extend the Dock to show application groups? In fact, why not make the Dock _the_ mechanism to show the Cast, instead of the previews on the left? Or, if you insist on keeping the Cast as thumbnails on the left, why not get rid of the Dock entirely? Adopt something from GNOME and put Launchpad behind a hotkey for launching apps.
* **Why not merge Spaces and Stage Manager?** Both of these features seem aimed at helping users manage lots of windows---why not merge them? Give us the ability to assign an application to an "App Group", where it will be grouped together with other applications in the Cast when it is launched (much in the same way you can assign an application to a Space).

I do believe that Stage Manager has the potential to be enormously useful. In its current form, though, it just feels incomplete. If you do decide to stick with it, though, Howard Oakley's [article on Stage Manager workflows][link-2] would be useful, I think.

Thanks for reading! Have opinions about what I've written? Contact me [on Twitter][link-3] or [on Mastodon][link-4] and tell me what you think!

[link-1]: https://eclecticlight.co/2023/01/11/stage-manager-for-the-unimpressed-1-getting-started/
[link-2]: https://eclecticlight.co/2023/01/13/stage-manager-for-the-unimpressed-2-workflow-strategies/
[link-3]: https://twitter.com/scott_lowe
[link-4]: https://fosstodon.org/@scottslowe
