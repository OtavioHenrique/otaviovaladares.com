---
layout: post
title: "Migrating from vim to neovim"
subtitle: "All steps to a peaceful migration"
date: 2018-09-30 00:28:00
author: "Otavio H. P. Valadares"
header-img: "img/in-post/migrating-from-vim-to-nvim/nvim.png"
header-mask: 0.5
catalog: true
multilingual: true
tags:
- Programming
- Software
- Tools
---

This week I decided to migrate to neovim, and give it a chance after a recommendation and some discussions about the future of vim text editor.

If you have a question about if you need to start using neovim I pretty recommend that you search on the internet and make your own decision. You can start by reading the following things:

- [Nice stackexchange question](https://vi.stackexchange.com/questions/34/what-is-neovim-how-is-it-different-from-vim-and-why-should-i-care)
- [Reddit thread](https://www.reddit.com/r/neovim/comments/52eu6b/how_does_vim8_compare_to_neovim/)

## Downloading

The first thing that I loved is the facility on download, at the official repository they provide a lot of good options for each OS, for linux, an `appimage file` and that is awesome, for who doesn't know it is like MAC OS applications file, for MAC OS you can download using brew and for windows you can download using chocolatey.

You can download neovim in the following two sites:

https://github.com/neovim/neovim/wiki/Installing-Neovim#install-from-download
https://github.com/neovim/neovim/releases/

As I described I choose the `appimage`, so I just downloaded and moved it to `/usr/local/bin` and all works fine, now just type `nvim` and it will open.

## Using the same setup of vim

The next question of everybody is probably "Ok, but I'll need to configure everything again? All my plugins?" and the answer is **No, you won't**.

If you're coming from vim like me, you can simply type `:help nvim-from-vim` and follow the instructions, simplifying is just create a file `~/.config/nvim/init.vim` that will be like your `.vimrc`, you'll paste all your custom config at this file.

```
touch ~/.config/nvim/init.vim
```

If you're a simple guy and just want to use your nvim in the same way as you use your vim, just paste the following commands at your `init.vim` that you created.

```
set runtimepath^=~/.vim runtimepath+=~/.vim/after
let &packpath = &runtimepath
source ~/.vimrc
```

It will route your nvim config to your previous vim setup, but it's important to know that some plugins may not work, nvim isn't entirely compatible with vim, but almost all plugins work on nvim too, with me, all plugins worked fine.

## Different setup for nvim

For a lot of people the previous solution will be satisfying, but for me no. I want to stop using vim, so the previous solution doesn't solve my problem, I want to have my own nvim setup. Luckily it's easy too, on vim I use `vim-pathogen` to manage my plugins, so all my plugins are located at `~/.vim/bundle/`, I just need to make a copy of this folder but on my nvim directory.

```
cp -r ~/.vim/bundle ~/.config/nvim/bundle/
```

And do the same with the `.vimrc` file.

```
cp ~/.vim/.vimrc ~/.config/nvim/init.vim
```

Now, if I want to modify something, I just need to modify on `init.vim` as you want, or install plugins at `~/.config/nvim/bundle`.

## Repository with nvim files

I like to create a Github repo to store my configs, so I just started a git repo at `~/.config/nvim` and pushed it, you can find my nvim setup [here](https://github.com/OtavioHenrique/nvimfiles).

*You could find my vim files [here](https://github.com/OtavioHenrique/vimfiles) but now with neovim, it's deprecated.*

## Nvim differences

Neovim has some differences from vim, you can check it by typing `:help vim-differences`, but the major features (copying from vim-differences output) is:

- API
- Lua scripting
- Job control
- Remote plugins
- Providers
   - Clipboard
   - Node.js plugins
   - Python plugins
   - Ruby plugins
- Shared data
- Embedded terminal
- VimL parser
- XDG base directories

But I pretty recommend that you read the full documentation at `:help vim-differences`.

Nvim have some plugins that take advantage of specific nvim features, you can check it [here](https://github.com/neovim/neovim/wiki/Related-projects)

## Final thoughts

It wasn't a very long post, and it's purposeful, I don't have much to talk about neovim yet, the objective of this post It only talks about how I migrate from vim to neovim. On next weeks I'll write about my plugins and describing some tricks of my vim/nvim configs.

If you have any question that I can help you, please ask! Send an email (otaviopvaladares@gmail.com), pm me on my [Twitter](https://twitter.com/opvaladares) or comment on this post!
