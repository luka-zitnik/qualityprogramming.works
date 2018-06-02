---
layout: post
title: Wrapping Git Commit Messages
subtitle: Minimal editor tweaking to help you with wrapping commit messages, and a bit of a reasoning about the 50/72 (50/80) wrapping rule
date: 2018-06-02 00:00:00 +0000
background: /images/alexandre-godreau-203580-unsplash.jpg
---

Unless you've specified a default editor through either `VISUAL` or `EDITOR` environment variable, *Git* will fallback to the *Vi* editor, commonly symlinked to *vim*.

We are going to tell *Vim* to place vertical guidelines at text editor column 51 and 76, and to hard wrap text at column 75, that is, insert newline into the current line of text when it exceeds 75 characters. And we are going to tell Vim to do that only when a commit message is being written.

But firstly, some things might already be working for you. Open Vim and type `:filetype`. If it says both filetype detection and filetype plugins are enabled, Vim is already hard wrapping your commit messages to 72 characters per line. That may be all the help you need from a git editor. But in case your formatting style says another number or you do want the guidelines (you don't think they are visually intrusive), follow these few other steps.

Make your user *vimrc file*, `$HOME/.vimrc`, and enable filetype detection and filetype plugin files support by writing `filetype plugin on` into it.

The plugin files allows us to place our Git commit-specific Vim configuration into a separate configuration file, `$HOME/.vim/ftplugin/gitcommit.vim`. Finally, with the following content of this other file:

```
set cc=51,76
set tw=75
```

Vim will highlight columns 51 and 76 with a special color (those are our suggested width boundaries, so to speak) and limit width of text that is being inserted to 75 characters per line (Vim will insert line breaks as we type).

This will obviously behave the same way for all of your projects. If for some reason you need different values for different projects, you can e.g. combine Vim's `autocmd` command and `-c` argument with Git's `core.editor` configuration option.

## On The 50/72 (AKA 50/80) Wrapping Rule

The rule says the first line of the commit message should not exceed 50 characters, the second line should be blank, and the rest of the commit message should not exceed the width of 72 characters.

The only argument I've seen so far of the 50-character limit is the average for the Linux kernel code repository, which must imply it's most reasonable to most people. And of the 72-character limit, it's long term support for 80-column text terminals – 80 characters, minus 4 for the whitespace that Git places on the left to pad the commit messages in the `git-log` command output, minus 4 to give it an even spacing to the right. And that's beautiful, almost poetic. Man, that goes back to 1928's 80-column punched cards. That's our history. True, but unless you are a fan of text terminals, following that rule on daily basis is also ludicrous. For one thing, when was the last time you've used an 80-column text terminal? Was it the *Command Prompt* in Windows 9? But even then you were customizing it with bigger window size (and switching to a smoother TrueType font). And besides, you were mostly using *Mintty* that shipped with Git or *Cygwin*. Am I right? There are better arguments in favour of the rule. So let's state them.

### Compliance With Current Tooling

The most basic insight, not to underestimate, is that we have to wrap the commit messages because `git-log` won't do it for us, i.e. won't do a soft wrap, because that's how it works. Another heavily used tool is GitHub's *Conversation* tab of a *Pull request*. It will soft wrap the lines extending over 76 characters but also keep your hard wrapping newlines. Fail!

### Readability

To the patient ones (I admire you), this is the most rewarding section of the article. It starts with two words. Typography studies! People skip words while reading (the fast eye movements are called *saccades*), they use their peripheral vision to find the next spot in the text to focus on (which contributes to the F-shaped reading pattern) and they accidentally read same lines twice (the phenomenon is called doubling). We can skim, scan or read more or less thoroughly. For the purpose of increasing reading speed and accuracy (increasing readability), and decreasing eye fatigue, researches don't quite agree one with another, but the proposition to keep line lengths between 45-75 characters is widely accepted among typographers. So is the 40-50 cpl (characters per line) measure for multi-column text, like the one we get with `git log --oneline`.

## References

- [Vim documentation: filetype](http://vimdoc.sourceforge.net/htmldoc/filetype.html)
- [Vim documentation: autocmd](http://vimdoc.sourceforge.net/htmldoc/autocmd.html)
- [Vim Tip: How to Use Vertical Guides](http://joshorourke.com/2012/06/29/vim-tip-how-to-use-vertical-guides)
- [Git - Git Configuration](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)
- [Keep your vimrc file clean \| Vim Tips Wiki \| FANDOM powered by Wikia](http://vim.wikia.com/wiki/Keep_your_vimrc_file_clean)
- [Configuring Git and Vim – CSS Wizardry – CSS Architecture, Web Performance Optimisation, and more, by Harry Roberts](https://csswizardry.com/2017/03/configuring-git-and-vim/)
- [Automatic wrapping of Git commit messages using Vim · wincent.com](https://wincent.com/blog/automatic-wrapping-of-git-commit-messages-using-vim)
- [Automatically wrap long Git commit messages in Vim - Stack Overflow](https://stackoverflow.com/questions/11023194/automatically-wrap-long-git-commit-messages-in-vim/11168851)
- [What’s with the 50/72 rule? – Preslav Rachev – Medium](https://medium.com/@preslavrachev/what-s-with-the-50-72-rule-8a906f61f09c)
- [tbaggery - A Note About Git Commit Messages](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)
- [Git Commit Messages : 50/72 Formatting - Stack Overflow](https://stackoverflow.com/questions/2290016/git-commit-messages-50-72-formatting)
- [history - Why is 80 characters the 'standard' limit for code width? - Software Engineering Stack Exchange](https://softwareengineering.stackexchange.com/questions/148677/why-is-80-characters-the-standard-limit-for-code-width)
- [Add support for AR5BBU22 [0489:e03c] by ReeJK · Pull Request #17 · torvalds/linux](https://github.com/torvalds/linux/pull/17#issuecomment-5661185)
- [Size Matters: Balancing Line Length And Font Size In Responsive Web Design — Smashing Magazine](https://www.smashingmagazine.com/2014/09/balancing-line-length-font-size-responsive-web-design/)
- [The Line Length Misconception \| Viget](https://www.viget.com/articles/the-line-length-misconception/)
- [Line length - Wikipedia](https://en.wikipedia.org/wiki/Line_length)
