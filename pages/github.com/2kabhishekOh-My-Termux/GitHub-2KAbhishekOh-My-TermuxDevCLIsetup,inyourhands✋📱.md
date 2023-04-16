---
title: GitHub - 2KAbhishek/Oh-My-Termux: Dev CLI setup, in your hands ✋📱
pageTitle: GitHub - 2KAbhishek/Oh-My-Termux: Dev CLI setup, in your hands ✋📱
length: 2041
author: 2KAbhishek
timestamp: 2023-04-16T15:04:09-05:00
markdownload-tags: []
markdownload-source: https://github.com/2kabhishek/Oh-My-Termux
markdownload-hostname: github.com
---

# GitHub - 2KAbhishek/Oh-My-Termux: Dev CLI setup, in your hands ✋📱

## Excerpt
> Dev CLI setup, in your hands ✋📱. Contribute to 2KAbhishek/Oh-My-Termux development by creating an account on GitHub.

---
## What is this

My personal configs, carefully and passionately crafted for setting up an optimal CLI dev experience on the go.

Bootstraps `termux` with `oh-my-zsh`, custom `p10k` prompt, `neovim`, `tmux`, `ranger` & `powerline` fonts and a bunch of other useful CLI tools and configs.

## Inspiration

Wanting a super portable CLI dev setup.

## Setup

```shell
git clone https://github.com/2kabhishek/Oh-My-Termux
cd Oh-My-Termux

# Menu based interactive setup
./setup.sh

# Setup everythin unattended
./setup.sh -a
```

## Requirements

For installation `git` `curl` & `zsh` are must, other tools are mentioned below.

### Packages

This list is incomplete, package names may vary depending upon your system and your requirements.

```shell
# Required
git, zsh, neovim, tmux, git-delta, bat, fd, fzf, fasd, ag(silver_surfer), curl, powerline, lsd
# Optional
ranger, cmus, htop, python, vim, broot, bash, ncdu, grc, exa, xplr
```

Powerline patched fonts are required for glyphs. I'll recommend [Nerd Fonts](https://www.nerdfonts.com/). I'm using FiraCode.

### Included Configurations

This repo contains configurations for following tools.

-   bash : [~/.bashrc](https://github.com/2KAbhishek/Oh-My-Termux/blob/main/.bashrc)
-   bat : [~/.config/bat/](https://github.com/2KAbhishek/Oh-My-Termux/blob/main/.config/bat/)
-   broot : [~/.config/broot/](https://github.com/2KAbhishek/Oh-My-Termux/blob/main/.config/broot/)
-   cmus : [~/.config/cmus/](https://github.com/2KAbhishek/Oh-My-Termux/blob/main/.config/cmus/)
-   git : [~/.gitconfig](https://github.com/2KAbhishek/Oh-My-Termux/blob/main/.gitconfig)
-   htop : [~/.config/htop/](https://github.com/2KAbhishek/Oh-My-Termux/blob/main/.config/htop/)
-   neovim : [~/.config/nvim/](https://github.com/2KAbhishek/Oh-My-Termux/blob/main/.config/nvim/)
-   python : [~/.pystartup](https://github.com/2KAbhishek/Oh-My-Termux/blob/main/.pystartup)
-   ranger : [~/.config/ranger/](https://github.com/2KAbhishek/Oh-My-Termux/blob/main/.config/ranger/)
-   tmux : [~/.tmux.conf](https://github.com/2KAbhishek/Oh-My-Termux/blob/main/.tmux.conf)
-   vim : [~/.vimrc](https://github.com/2KAbhishek/Oh-My-Termux/blob/main/.vimrc)
-   zsh : [~/.zshrc](https://github.com/2KAbhishek/Oh-My-Termux/blob/main/.zshrc)

## How it was built

Oh-My-Termux was built using bits and pieces from my [Dotfiles](https://github.com/2kabhishek/Dotfiles), repurposed and tweaked to fit this use case

## Challenges faced

Finding workarounds for a few things on `termux` was a bit challenging.

## What I learned

I learned about writing good cross-platform bash scripts and learned a lot about making flexible configs and making them as easy to setup as possible.

## What's next

Planning to add more tools, better management, always updating.

Hit the ![star][fig1] button if you found this useful.

## More Info

Want the complete CLI experience on your nix system too? Have a look at [Dotfiles](https://github.com/2kabhishek/Dotfiles)

Looking for my `neovim` configs? See [nvim](https://github.com/2kabhishek/nvim)

Looking for my `tmux` configs? See [tmux2k](https://github.com/2kabhishek/tmux2k)

Find more cool configs and setups at [GitHub does Dotfiles](https://dotfiles.github.io/)

[fig1]: https://github.githubassets.com/images/icons/emoji/unicode/2b50.png

> Saved from https://github.com/2kabhishek/Oh-My-Termux on 2023-04-16T15:04:09-05:00