---
title: "Vim Setup From Scratch"
date: 2024-10-14T09:31:35+08:00
description: "A guide to setup and configure your Vim installation from scratch"
tags: ["programming", "vim"]
featured_image: "/images/vim.jpg"
---

Vim setups can be quite involved. There is the batteries included approach (SpaceVim, LazyVim etc.) and there's the hand rolled approach. Although the readymade approach is easier, manually setting up your Vim configuration can be a good learning experience.

This guide is a way for me to have all the steps I normally follow when installing Vim. I'll try and keep it simple and focus on languages I enjoy using as a developer - Golang and Python.

This guide will also focus on MacOS as I am using an M1 Macbook Pro at the moment. Most of the steps can be easily translated to other operating systems.

## Installation

Install Vim by issuing the following command -

```bash
brew install vim
```

## Sensible defaults

The sensible defaults plugin is a great way to have a starting point with your .vimrc (Vim's configuration file) without having to copy over someone else's configuration file. 

However, this is a plugin which will have to be installed using the steps in section 'Plugin Manager'

## Plugin Manager

One of the easiest ways to set things up in Vim and install plugins is to use a plugin manager. We'll use vim-plug here in our example -

```bash
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
```
Add a vim-plug section to your ~/.vimrc (or ~/.config/nvim/init.vim for Neovim)

```bash
call plug#begin()

" List your plugins here
Plug 'tpope/vim-sensible'

call plug#end()
```
Reload the file or restart Vim, then you can,
`:PlugInstall` to install the plugins
`:PlugUpdate` to install or update the plugins
`:PlugDiff` to review the changes from the last update
`:PlugClean` to remove plugins no longer in the list


## Useful developer plugins

Below is a list of useful plugins to install in Vim -

* Vim sensible - A set of sensible defaults for your .vimrc
* NERDTree - File explorer
* fzf.vim - Fuzzy file finder
* ALE - Linting and static analysis
* vim-fugitive - Git integration

Add these between the call plug#begin() and call plug#end() sections of your .vimrc -

```bash
Plug 'tpope/vim-sensible'
Plug 'scrooloose/nerdtree'
Plug 'junegunn/fzf'
Plug 'w0rp/ale'
Plug 'tpope/vim-fugitive'
```

Use the `:PlugInstall` command to install above plugins.

## Language support

I am tailoring this guide for Golang as it is my preferred programming language for now. Install the following plugins, by modifying your ~/.vimrc file -

```bash
Plug 'fatih/vim-go'
Plug 'vim-python/python-syntax'

```

## Colour scheme

Choose a color scheme that's easy on the eyes for long coding sessions. Molokai is a popular choice -

### Download

```bash
curl -fLo ~/.vim/colors/monokai.vim --create-dirs https://raw.githubusercontent.com/crusoexia/vim-monokai/refs/heads/master/colors/monokai.vim
```

### Installation

Select the theme by adding the following lines to your ~/.vimrc

```bash
colorscheme monokai
```
## Key mappings

You can add custom keymappings, such as invoking NERDTree, by adding the following to your ~/.vimrc

```bash
nnoremap <C-n> :NERDTreeToggle<CR>
nnoremap <C-p> :FZF<CR>
```

## Go specific plugins

```bash
Plug 'fatih/vim-go', { 'do': ':GoUpdateBinaries' }
Plug 'neoclide/coc.nvim', {'branch': 'release'}
Plug 'SirVer/ultisnips'
Plug 'AndrewRadev/splitjoin.vim'
```

* vim-go: The essential plugin for Go development in Vim
* coc.nvim: For advanced code completion (install gopls separately)
* ultisnips: For code snippets
* splitjoin.vim: Useful for splitting/joining struct literals

## Go specific settings

Use the following settings for an optimal Go programming experience -

```bash
" Go syntax highlighting
let g:go_highlight_fields = 1
let g:go_highlight_functions = 1
let g:go_highlight_function_calls = 1
let g:go_highlight_extra_types = 1
let g:go_highlight_operators = 1

" Auto formatting and importing
let g:go_fmt_autosave = 1
let g:go_fmt_command = "goimports"

" Status line types/signatures
let g:go_auto_type_info = 1

" Run :GoBuild or :GoTestCompile based on the go file
function! s:build_go_files()
  let l:file = expand('%')
  if l:file =~# '^\f\+_test\.go$'
    call go#test#Test(0, 1)
  elseif l:file =~# '^\f\+\.go$'
    call go#cmd#Build(0)
  endif
endfunction

" Map keys for most used commands.
" Ex: `\b` for building, `\r` for running and `\b` for running test.
autocmd FileType go nmap <leader>b :<C-u>call <SID>build_go_files()<CR>
autocmd FileType go nmap <leader>r  <Plug>(go-run)
autocmd FileType go nmap <leader>t  <Plug>(go-test)
```

Remember to run :GoInstallBinaries after setting this up to install necessary Go tools. Also, ensure you have gopls installed (go install golang.org/x/tools/gopls@latest) for the best code completion experience with coc.nvim.

This should give you a solid starting point for your Vim install. You can visit a website like https://vimawesome.com/ to get inspiration for plugins and further customisation.

