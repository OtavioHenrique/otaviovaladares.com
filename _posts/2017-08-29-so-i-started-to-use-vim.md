---
layout:     post
title:      "So, I started to use Vim"
sybtitle:   "and it isn’t too hard"
date:       2017-08-29 00:00:00
author:     "Otavio Henrique"
header-img: "img/in-post/functional-programming/fp-bg.jpeg"
catalog:    true
tags:
    - Opinion
    - Programming
    - Software
    - Tools
---

![](https://cdn-images-1.medium.com/max/800/1*ro1QoXvKwQjgl0WCq5KZEQ.png)

Since I started studying programming, I have heard about use Vim to code, but I
have always used IDEs such as Visual Studio, CLion, IntelliJ, Eclipse, NetBeans,
or text editor like Sublime and Atom (what I’m using in the last two years).
When I had a look at vim tutors, or people using vim, I always thought, “That’s
is so hard”, “I will never learn this”, “Look at this shortcuts”, “How I start
using this?”, but in the last two months a several reasons made me to start
learning and using Vim.

#### Vim is a light and fast text editor

Today almost all text editors like Atom with a tons of Js, sublime text and VS
Code just fuck off your computer memory, and I’m not even going to talk about
IDEs because anybody knows how much they consume your computer memory (And I’m
not a person that is against using IDEs, in some kind of development it is good,
like Android development for example).

You can check more on [this fantastic
post](https://medium.freecodecamp.org/why-i-still-use-vim-67afd76b4db6) and in
[this
repository](https://github.com/jhallen/joes-sandbox/tree/master/editor-perf).

#### Vim is a extensible text editor.

Everybody know, you can easily adapt vim for whatever you want to do, you can
install the plugins that you want for power up your productivy.

#### Use everywhere

You can use Vim everywhere, yes, vim is cross-plataform, almost all operating
systems supports vim, and others editor like nano no. And plus, you can use vim
mode in a lot of editors/IDE’s like, emacs, sublime, xcode, eclipse etc.

![](https://cdn-images-1.medium.com/max/800/1*9LsLtjuX0HuZZcHM7kxQHA.jpeg)
<span class="figcaption_hack">Vim is cross-plataform</span>

Mainly because of these reasons I decided to Start using Vim.

#### Where did I started?

Like most of people when I started, I decided to start using an already
configurated Vim, like, [YADR](https://github.com/skwp/dotfiles) and [The
ultimate Vim configuration](https://github.com/amix/vimrc), these are awesome
vim, I tried for few weeks on my personal projects, but when I was using them I
felt lost in plugins, navigation, shortcuts and I have spent a lot of time
searching on google how to do simple things, searching for understanding what
that plugin do.

So I decided to start from zero installing Gvim ([you can find the difference
between gvim, vim-gtk, etc
here](https://askubuntu.com/questions/281886/what-are-the-differences-between-the-different-vim-packages-available-in-ubuntu))
and searching for plugins that I need, the colorscheme that I like among others,
what happened? I started to like Vim, I started with raw Vim, and as necessary I
went on installing my plugins and learned new tricks and shortcuts.

Which plugins have I installed on the beginning? Heres a list of them:

* [ctrlp.vim](https://github.com/kien/ctrlp.vim) (I can’t live without ctrl+p
command of other text editors, and heres a plugin to do this on vim)
* [nerdtree](https://github.com/scrooloose/nerdtree) (The legendary tree explorer)
* [vim-polyglot](https://github.com/sheerun/vim-polyglot) (Collection of language
packs for almost all languages)
* [grep.vim](https://github.com/vim-scripts/grep.vim) (Searching tool for Vim)
* [vim-rails](https://github.com/tpope/vim-rails) (I installed this because I’m
studyng Rails but its not a “Essencial plugin”)
* [fzd.vim](https://github.com/junegunn/fzf.vim) (To use global search (Using
[the_silver_searcher](https://github.com/ggreer/the_silver_searcher)))

I installed all these plugins using
[pathogen.vim](https://github.com/tpope/vim-pathogen) because I found it more
simpl.

Finally, I spent less than an hour and already had a Vim to start coding and
learning. After a few days I installed some other plugins, but for the start
what I have mentioned is enough and Ipretty reccomend this aprproach for
everyone that want to start and don’t know where or how to start. With this
method you will learn the plugins you’re installing and the shortcut while you
are working on your personal projects, and when you feel safe developing using
Vim, you can start using it on your job.

If you want, you can watch some vim casts to improve your ability at
[http://vimcasts.org/](http://vimcasts.org/), it’s very good quality.

If you want, you can find my vimfiles
[here](https://github.com/OtavioHenrique/vimfiles), and its all documented.

Thanks.

That’s all for today folks, don’t forget to follow my blog and my
[twitter](https://twitter.com/ValadaresOtavio).
