---
title: Your First vimrc Should Be Nearly Empty
layout: post
---

A new user's vimrc should be as minimal as possible.

Many folks get excited about pre-configured setups, such as Janus, with a
huge vimrc and numerous plugins already installed. This a handy shortcut to a
feature-rich experience, but using this sort of package will actually slow your
long-term mastery of the editor.

For the first few weeks, you should be getting acquainted with the editor
with the bare minimum of customizations. This will let you learn vim as it is
out-of-the-box. Don't complicate an already challenging learning process by
adding in the complexities of plugins and extensive customization.

Additionally, resist the urge to tweak at first. If you start changing settings
you're likely to trample over some sensible defaults that don't make sense to
you yet because of your novice status.

I wish I could recommend that you start with no vimrc at all. However, running
vim with no vimrc puts it in compatibility mode, where it tries to retain
compatibility to its inferior precursor, vi. As such, **you should start with
[this minimal vimrc](https://gist.github.com/r00k/8fc7e4e9d35ccbfb64aa)** that I
created for you. It's extremely minimal, setting only the most vital options
that should have been enabled by default anyhow. Additionally, it's
well-commented and should be easily understandable.

Once you have a few weeks of vim under your belt, feel free to start tweaking
things. But do it slowly. Customizations should be accreted&mdash;built up
bit-by-bit over a long period of time. A good vimrc grows linearly with your
skill.

When you do add something to your vimrc, make sure you understand every
character of the addition you make. If you don't, it's time to type `:h
setting` and read up on it.

The same goes for plugins. Once you've gotten the hang of the basics, feel
free to take a few for a spin, but make sure to read their documentation so you
know what you've changed.

Finally, while simply dumping someone else's vimrc into yours is a bad idea,
mining a more-experienced user's file for clever hacks is a fantastic one. Pick
one or two things you've found, read the help on them, and add them to your
carefully-tended config file. And don't forget the comments.
