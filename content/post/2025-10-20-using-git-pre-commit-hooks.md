---
author: slowe
categories: Explanation
comments: true
date: 2025-10-20T09:00:00-06:00
tags:
- CLI
- Git
- Markdown
title: Using Git Pre-Commit Hooks
url: /2025/10/20/using-git-pre-commit-hooks/
---

A while ago I wrote an article about [linting Markdown files][xref-1] with `markdownlint`. In that article, I presented the use case of linting the Markdown source files for this site. While manually running linting checks is fine---there are times and situations when this is appropriate and necessary---this is the sort of task that is ideally suited for [a Git pre-commit hook][link-3]. In this post, I'll discuss Git pre-commit hooks in the context of using them to run linting checks.<!--more-->

Before moving on, a disclaimer: I am not an expert on Git hooks. This post shares my limited experience and provides an example based on what I use for this site. I have no doubt that my current implementation will improve over time as my knowledge and experience grow.

## What is a Git Hook?

As [this page][link-6] explains, a hook is a program "you can place in a hooks directory to trigger actions at certain points in git's execution." Generally, a hook is a script of some sort. Git supports different hooks that get invoked in response to specific actions in Git; in this particular instance, I'm focusing on the pre-commit hook. This hook gets invoked by `git-commit` (i.e., the user running a `git commit` command) and allows users to perform a series of checks or tests before actually making a commit. If the pre-commit script exits with a non-zero status, then Git aborts the commit.

## Using a Pre-Commit Hook to Lint Markdown

For my use case, I wanted to run Markdownlint to lint the Markdown files (as described in [the previous article on linting Markdown][xref-1]) before the commit. To do that, I came up with the following script:

```bash
#!/usr/bin/env bash

MDLINT="/usr/local/bin/markdownlint"

for file in $(git diff --cached --name-only --diff-filter=ACM | grep "\.md"); do
    if ! $MDLINT "$file"; then
        echo "Lint check failed on file '$file'."
        echo "Run markdownlint to identify the errors and try again."
        exit 1
    fi
done
```

To make this script active as a pre-commit hook, you must name it `pre-commit`, you must place it into Git's hook directory (which defaults to `.git/hooks` in the current repository), and you must make it executable.

This script is readable enough for most folks to understand what it is doing, but there are a couple of things that might be helpful to point out:

* The `git diff --cached --name-only` command will return a list of the files staged for commit using `git add`. This list of files is what will Markdownlint will check.
* The `--diff-filter=ACM` parameter tells `git diff` to return only Added, Copied, and Modified files. This prevents `git diff` from returning the filename of a deleted file, which would then generate an error from Markdownlint.
* Using `grep` here further restricts the list of files to include only Markdown files.
* The `exit 1` command is critical; without returning a non-zero exit code, Git wouldn't know to abort the commit.

With the script in the right place and marked as executable, it's automatically invoked by Git when I try commit any Markdown files. If the check succeeds, the commit proceeds as normal (my editor opens for me to edit the commit message); if the check fails, then I see an error message and the commit aborts. Exactly what I needed!

## Additional Resources

I found the following articles useful when I was learning about pre-commit hooks and how to use them:

[How to use git pre-commit hooks, the hard way and the easy way][link-1]

[Git Pre-Commit Hook: A Practical Guide (with Examples)][link-2]

I hope you've found this post helpful in some fashion. As I previously mentioned, I don't pretend to be an expert on this topic. If you spot an error or mistake in this post, then please reach out and let me know. You can reach me [on Twitter][link-4], on [the Fediverse][link-5], or in a variety of Slack communities. Positive feedback is also welcome---thanks for reading!

[link-1]: https://verdantfox.com/blog/how-to-use-git-pre-commit-hooks-the-hard-way-and-the-easy-way
[link-2]: https://www.slingacademy.com/article/git-pre-commit-hook-a-practical-guide-with-examples/
[link-3]: https://git-scm.com/docs/githooks#_pre_commit
[link-4]: https://twitter.com/scott_lowe
[link-5]: https://fosstodon.org/@scottslowe
[link-6]: https://git-scm.com/docs/githooks
[xref-1]: {{< relref "2024-03-01-linting-your-markdown-files.md" >}}
