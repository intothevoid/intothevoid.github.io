---
title: "Setup Vim (Astro Vim) on a Macbook"
description: "Setup a neovim instance on a Macbook M1/M2 ready for a development workflow."
tags: ["vim", "macos", "m1"]
date: 2022-07-31T11:53:48+09:30
featured_image: "images/vim-logo.jpg"
---

## Overview

Use the following steps to have a decent vim installation relatively quickly on a Macbook with a M1 / M2 chipset

## Brew installation

Brew is a package manager for MacOS which makes it very easy to install packages. I highly recommend this package manager on MacOS. Its worth the effort to get it installed you will thank yourself later on.

From www.brew.sh -

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Sample command using brew -

```bash
brew install wget
```

## Install NeoVim

Neovim is our base vim install. Install Neovim using the command -

```bash
brew install neovim
```
## Install Astro Vim

AstroNvim is an aesthetic and feature-rich neovim config that is extensible and easy to use with a great set of plugins. Install AstroNvim by using the commands

```bash
git clone https://github.com/AstroNvim/AstroNvim ~/.config/nvim

nvim +PackerSync
```

## Setup Astro Vim

Make a backup of your existing nvim configuration -

```bash
mv ~/.config/nvim ~/.config/nvimbackup
```

#### Install LSP

Enter `:LspInstall` followed by the name of the server you want to install  
Example: `:LspInstall pyright`

#### Install language parser

Enter `:TSInstall` followed by the name of the language you want to install  
Example: `:TSInstall python`

#### Manage plugins

Run `:PackerClean` to remove any disabled or unused plugins  
Run `:PackerSync` to update and clean plugins

#### Update AstroNvim

Run `:AstroUpdate` to get the latest updates from the repository

#### Install NerdFonts

Follow this step to install your desired fonts and so font glyphs are rendered correctly

Firacode is my current favourite ♥️

Fonts can be downloaded from https://www.nerdfonts.com/font-downloads

#### Optional requirements

These are other optional packages which can be installed -

[ripgrep](https://github.com/BurntSushi/ripgrep "ripgrep") - live grep telescope search (leader + fw)

[lazygit](https://github.com/jesseduffield/lazygit "lazygit") - git ui toggle terminal (leader + tl or leader + gg)

[NCDU](https://dev.yorhel.nl/ncdu "NCDU") - disk usage toggle terminal (leader + tu)

[Htop](https://htop.dev/ "Htop") - process viewer toggle terminal (leader + tt)

[Python](https://www.python.org/ "Python") - python repl toggle terminal (leader + tp)

[Node](https://nodejs.org/en/ "Node") - node repl toggle terminal (leader + tn)


