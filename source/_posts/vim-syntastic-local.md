---
title: Setup local eslint for JavaScript for VIM with syntastic
tags:
  - JavaScript
  - vim
  - eslint
  - syntastic
categories:
  - Tools
  - VIM
date: 2016-10-06 18:01:21
---


{% asset_img "title.png" Vim + Syntastic + local eslint %}


# Use one of plugin managers

<a href="https://github.com/junegunn/vim-plug/">Vim-Plug</a> <br>
<a href="https://github.com/Shougo/neobundle.vim">NeoBundle</a> <br>
<a href="https://github.com/gmarik/Vundle.vim">Vundle</a> <br>
<a href="https://github.com/tpope/vim-pathogen">Pathogen</a> <br>
<a href="https://github.com/MarcWeber/vim-addon-manager">Vim-Addon-Manager</a> <br>

# Install plugins

### Add lines in .vimrc

```
Plug 'scrooloose/syntastic'
Plug 'mtscout6/syntastic-local-eslint.vim'
```

### Run below commands to install those two plugins in vim.  
First one applies changes to vim while you are opening .vimrc file.  
Second does installing plugins. (this may differ by the manager)

```
:so %
:PlugInstall 
```

# Install Eslint as a checker for JavaScript

Order matters here.

Please make sure remove global eslint first.  

Install eslint in local repo

```
npm i -D eslint
```

Then, install eslint-cli package in global scope.

```
npm i -g eslint-cli
```

Make sure that you can still run `eslint` but it will run local eslint instead.

Now let's configure the syntastic in .vimrc
There are tons of options in syntastic but this may be a good start.

```
" syntactic
set statusline+=%#warningmsg#
set statusline+=%{SyntasticStatuslineFlag()}
set statusline+=%*

let g:syntastic_always_populate_loc_list = 1
let g:syntastic_loc_list_height = 5
let g:syntastic_auto_loc_list = 1
let g:syntastic_check_on_open = 1
let g:syntastic_check_on_wq = 1
let g:syntastic_javascript_checkers = ['eslint']

let g:syntastic_error_symbol = '❌'
let g:syntastic_style_error_symbol = '⁉️'
let g:syntastic_warning_symbol = '⚠️'
let g:syntastic_style_warning_symbol = '💩'

highlight link SyntasticErrorSign SignColumn
highlight link SyntasticWarningSign SignColumn
highlight link SyntasticStyleErrorSign SignColumn
highlight link SyntasticStyleWarningSign SignColumn
```

# init eslint for repository

```
eslint --init
```

Follow the instructions and answer the questions.

Now you are all set.

{% asset_img "example.png" Example for Vim + Syntastic + local eslint %}

