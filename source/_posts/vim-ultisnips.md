---
title: Syntax-agnostic snippets for Ultisnips
tags:
  - JavaScript
  - vim
  - Ultisnips
  - snippets
categories:
  - Tools
  - VIM
date: 2017-07-12 19:01:21
---



{% asset_img "title.jpg" ultisnips javascript, opting a semicolon %}

* This post is for users who uses
  * vim or neovim
  * Ultisnips
  * one of plugin managers

# Motivation
  When I work on multiple javascript projects such as open-source projects or my own projects, I need to accommodate my editor setup for each different setup and coding style guide with snippet plugins.

  For example, one of the projects uses style guide which requires a trailing semicolon at the end of statement. On the other hand, other projects prohibits it.

  Coding style checker for eslint can be setup differently with .eslintrc in root folder of each project, but editors like vim or neovim with plugins are not agnostic enough to accommodate and work according to those styles.  

  So I used to have something like this in my Ultisnip snippet files. With comma, without comma.  

  ```
  snippet log
  console.log(${1:${VISUAL}})
  endsnippet
  ```

  ```
  snippet log;
  console.log(${1:${VISUAL}});
  endsnippet
  ```

  This works, but it gets repetitive.  

  So I tried to figure out how I can achieve this easily.  

  It can be done like this.  
  [Ultisnip javascript](https://github.com/honza/vim-snippets/blob/master/UltiSnips/javascript.snippets#L11-L15)  

  ```
  global !p
  from javascript_snippets import (
    semi, space_before_function_paren, keyword_spacing
    )
  endglobal
  ```

  ```
  snippet ret "Return statement"
  return ${VISUAL}$0`!p snip.rv = semi(snip)`
  endsnippet
  ```

  **This happens in this order.**

  1. Define ultisnip variable in `.vimrc`. Vim will pick this up.
  ```
  let g:ultisnips_javascript = {
    \ 'keyword-spacing': 'always',
    \ 'semi': 'never',
    \ 'space-before-function-paren': 'always',
    \ }
  ```
  2. Ultisnip plugin picks that options up and has them available in ultisnip files.  
  3. When snippet is being called, it prints out semicolons based on the option.  

  This looks great but what if I want to set this up differently per project?  

  This is where `vim-localrc` comes into play.  

  [vim-localrc](https://github.com/thinca/vim-localrc)
  ```
  .
  ├── .local.vimrc
  └── folder1
      ├── .local.vimrc
      └── folder2
          └── .local.vimrc
  ```

  If we have this folder structure in the project, the most nested .local.vimrc gets precedence sequentially. So we can put any configs in any level of the folder structure.

  Here's how you can set it up.

# Install plugins

### Add lines in .vimrc
```
Bundle 'thinca/vim-localrc'
```

### Run below commands to install the plugin in vim.  

First one applies changes to vim while you are opening .vimrc file.  

Second does installing plugins. (this may differ by the manager)

```
:so %
:PluginInstall 
```

# Configs

### Put global config

`~/.vimrc` for vim
`~/.config/nvim/init.vim` for neovim

```
let g:ultisnips_javascript = {
  \ 'keyword-spacing': 'always',
  \ 'semi': 'never',
  \ 'space-before-function-paren': 'always',
  \ }
```

### Put local config for the project to use a different setup

`${PROJECT_DIR}/.local.vimrc`

```
let g:ultisnips_javascript = {
  \ 'semi': 'always',
  \ }
```

# Custom snippets

### my own snippets for javascript

* it's usually in `~/.vim/UltiSnips/javascript.snippets`

put this at the top of the file

```
global !p
from javascript_snippets import (
	semi, space_before_function_paren, keyword_spacing
)
endglobal
```

Use it like this

```
snippet log
console.log(${1:${VISUAL}})`!p snip.rv = semi(snip)`${0}
endsnippet
```

Happy coding! 🖥  🍺
