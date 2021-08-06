---
layout: default
title: Why I use Doom Emacs
excerpt: Doom Emacs broadens my editor horizons
---

# Me vs. Emacs

GNU Emacs is (and has been since 1985) a text editor that has always focused on
extreme customizability. Users do anything from editing code to writing books
to reading email to browsing the web to playing Tetris.

I have been using Emacs as my main text editor for over 10 years now mainly
due to the fact that I can make it behave **EXACTLY** the way I want.
For a long time there wasn't really another choice if you wanted a truly
customizable editor (VIM counts but more on that later). In the last few years,
VScode has actually changed this a little bit allowing all sorts of extensions
to make it a pretty customizable editor.

Once in a while the question comes up: Why would you use/learn Emacs if you can just
start with VScode?

Looking at VScode made me revisit a question I had not asked in a long time:
Could I (in theory) switch to a different editor if I really wanted.
The answer I came up with is **NO**, mainly because I realized how absurdly dependent I am on my 10 year old Emacs
configuration that has organically grown to a something that might come alive or
turn into SkyNet at any time.

This realization pushed me to experiment with simplifying or at least
standardizing my configuration a tiny bit such that I wouldn't be completely
helpless if I were ever forced to delete all
traces of my personal Emacs configuration.

I went through a few strategies:

## Trying to be more disciplined

The first thing I tried was fairly simple: Just be disciplined and use standard
Emacs keybindings and default packages settings more.

Unfortunately this failed miserably as the Emacs keybindings are not very
ergonomic (for me) on a modern squashed laptop keyboard and aren't very intuitive (to me).
In addition my personal keybindings were sometimes close "enough" to the originals that I would end up getting confused all the time.
I got lazy and gave up pretty quickly.

## Use someone else's Emacs configuration

There a bunch of people who spend a great amount of time configuring Emacs and
have published their configurations on GitHub. I could just go there grab one
and stick to it (or at least not mess it up).
A notable example in this category would be:

- https://github.com/purcell/emacs.d

Steve Purcell's personal emacs config in which he basically takes the "disciplined" approach mentioned above, but did it much, much better than I could ever do.

In the end, I decided to not go this route as I think it I would end up just morphing it into my broken unmaintainable strange configuration again (see the discipline problem above).

## Pre-bundled Emacs "Distributions"

These are various "meta-configurations" that pre-configure Emacs with (hopefully)
sensible settings and often bundle a few packages to give Emacs a modern look
and feel. Notable mentions here are:

- [Prelude](https://github.com/bbatsov/prelude): Nice configuration with a framework feeling to it, doing a lot of work for you and separating out a "personal" configuration folder to allow the user to leave the core prelude settings alone.

- [Spacemacs](https://www.spacemacs.org/): True magic framework.
This does everything for you, looks awesome right from the start, has all sorts of stuff baked in and is easy to start on as it adds a whole meta-configuration system on top called
"layers". It also uses vim keybindings (evil-mode).

- [Doom Emacs](https://github.com/hlissner/doom-emacs): Seems to be inspired by Spacemacs (also uses vim keybindings) but has a different approach regarding customization.
I ended up choosing doom because the defaults were good, it's was easy to add what, just as easy to opt-out of what I don't want and at the same time possible to extend and modify the stuff that doesn't work for me.

# Diving into Doom Emacs

Doom emacs is written by [Henrik Lissner](https://github.com/hlissner) and it's pretty amazing what he has done.
The configuration system is very clean and let's the user go from having a nice out of the box no-configuration experience (including a walkthrough setup wizard) to integrating your own Elisp code without breaking anything.

```
cp ~/.emacs.d ~/.emacs.d.backup # backup your own configuration if you have one
git clone --depth 1 https://github.com/hlissner/doom-emacs ~/.emacs.d
~/.emacs.d/bin/doom install
```

The generated file `~/.doom.d/init.el`, which looks like [this](https://github.com/hlissner/doom-emacs/blob/develop/init.example.el), is the only file you really have to edit if you're getting started.
Just uncomment the parts you want and run

```sh
~/.emacs.d/bin/doom refresh
```

which will download and configure the correct packages so the next time you
start emacs they will be there.

Regarding other configuration files, the `package.el` file defines what external packages need to be loaded and the
`config.el` is basically a free for all configuration file similar to the
`init.el` we're used to in Emacs.

The setup forces the me to think about the structure of [my configuration](https://github.com/daviskirk/doom.d).

## Turning evil and learning vim keybindings

Learning Vim keybindings actually has a lot of advantages:

- Vim Keybindings actually have an internal (if somewhat convoluted) logic to
  them [verbs, nouns and
  modifiers](https://yanpritzker.com/learn-to-speak-vim-verbs-nouns-and-modifiers-d7bfed1f6b2d),
  so it's not as common to change every single navigation key binding as it is in Emacs land.
- Vim or Vi is everywhere on basically every server and on every (unix) desktop.
- (Almost) every application related to software development has vim keybindings
  (I have the feeling Emacs bindings aren't quite as ubiquitous since so many
  Emacs users rebind the default Emacs bindings anyway)

The big advantage in using doom Emacs (or emacs + evil-mode in general) is that you still don't have to give up all of Emacs other customization possibilities and features.
I still get to use [Magit](https://magit.vc/), Docker and Kubernetes integration, org mode and all the good stuff I like about Emacs in the first place.

# Conclusion

I don't know if I can stick with the "default" Doom Emacs configuration or if I will end up in the same place I was a couple of months ago again.
I do think I will be able to stick to the basic Vim navigational bindings which will at least help me navigate other computers and servers (where I do not have my configurations installed) more successfully.

I get the feeling that modern configurable editors like VScode have inspired the Emacs community to think a little bit about "out of the box" usability for people that are used to using younger editors like VScode or Atom, and I'm glad about this.
After all, there are still enough reasons to use Emacs over any other editor and it would be a shame if anyone lost out because default Emacs scared them away on the first day.
