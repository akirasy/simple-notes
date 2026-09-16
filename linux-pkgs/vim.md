---
title  : vim
layout : default
parent : Linux Packages
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Config files

1. `.vimrc`

    ```
    " -- global config --
    syntax on
    filetype plugin indent on
    set ruler
    set number
    set nowrap
    set noswapfile
    ```

## Vim Add-ons

1. Create folders to put vim add-ons

    ```
    mkdir -p ~/.vim/pack/git/start
    ```

2. Clone the add-ons to the folder

    ```
    git clone --depth=1 https://github.com/username/repository.git ~/.vim/pack/git/start/{repository}
    ```

### Favorite vim Add-ons
- [emmet-vim](https://github.com/mattn/emmet-vim)
- [indentLine](https://github.com/Yggdroot/indentLine)