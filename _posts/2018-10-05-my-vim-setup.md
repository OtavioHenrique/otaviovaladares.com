---
layout: post
title: "My nvim/vim setup"
subtitle: "Explaining each nvim config that helps me on my daywork"
date: 2018-10-03 00:28:00
author: "Otavio Valadares"
header-img: "img/in-post/migrating-from-vim-to-nvim/nvim.png"
header-mask: 0.5
catalog: true
multilingual: true
tags:
- Programming
- Software
- Tools
---


## Plugins

Today I use 22 plugins, I don't use a lot of specific plugins, when I need something more scoped to one thing I  install but if I stop using this plugin I usually delete it. One example of specific plugin that I use is `vim-rails` that I only use because 80% of my daywork time is using rails. 

My current `bundle` directory contains the fallowing plugins:

- ag.vim
- ale
- auto-pairs
- comfortable-motion.vim
- ctrlp.vim
- incsearch.vim
- nerdcommeter
- nerdtree
- syntastic
- vim-airline
- vim-aitline-themes
- vim-endwise
- vim-fugitive
- vim-gitgutter
- vim-hardtime
- vim-markdown
- vim-orbital
- vim-polyglot
- vim-rails
- vim-resize
- vim-rubocop

### ag.vim

The first thing about `ag.vim` is that for me, it's a essencial plugin for every people that use nvim/vim, for who doesn't know, `ag.vim` is a global project search for a given word/pattern, he is like grep/ack but more faster, a lot of people still using grep/ack and it's ok, all the three tools do the same job, but ag.vim is faster.

To use `ag.vim` you need to have `the_silver_searcher` installed, and to install it is very easy, you can install using your package manager or by source.

After installing it and the `ag.vim` plugin it's just start hacking, to use it's very simple, just type:

```
:Ag <PATTERN>
```

And Ag will return all matching patterns in the project.

### ale

### auto-pairs

Auto pairs I don't have enought to explain, the only function of this plugin is to auto pairs parenthesis/brackets/curly.

### Comfortable-motion.vim

### ctrlp.vim

One of the most essencial thing in any text edit/IDE for me it's the `ctrl+p` command, I think every developer use this on daywork, the function is to search for a file in the project, and this plugin implements it on a beautiful way, just press `ctrl+p` and have fun!

### incsearch.vim

### nerdtree

nerdtree doesn't needs no introduction, it's the tree navigator, and I think every vim needs it too. Just install and have fun, nerdtree have a nice tricks, but I'll explain it at next part when I'll talk about my ´.vimrc/init.vim´.

### syntastic
 
 This plugin is another that I judge essencial, it's a huge plugin that implement syntax and syntax checker for a bunch of languages, for example, I don't need to have ´vim-ruby´ installed, it already highlight my ruby syntax and check for errors, it's a awesome plugin, if I start programming haskell tomorrow, syntastic already have haskell syntax installed. It has more than 100 languages. The custom setup that I used it's the basic recommended by them  at the doc:

### vim-airline

vim-airline it's not a must have plugin but I think everbody that use vim use it.

