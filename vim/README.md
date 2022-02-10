# Introduction

This document has various tidbits about vim and vim-latex that might be useful to other people trying to tune their settings. As of Feb. 2022, I am using [MacVim](https://github.com/macvim-dev/macvim) with [VIM-LaTeX](http://vim-latex.sourceforge.net/).

# Beamer 

## Adding frame environment

### Method 1: Environment
1. Add following to ``~/.vimrc``: ``let: g:Tex_Env_frame = "\\begin{frame}\<CR>\\frametitle{<++>}\<CR><++>\<CR>\\end{frame}<++>``
2. In the instance of vim, type ``:source $MYVIMRC`` or restart your vim instance.
3. To activate the environment, type ``frame`` and then ``F5`` (may also have to press the ``FN`` key).

### Method 2: Three Letters
1. Add following to ``~/.vimrc``: ``call IMAP('EFE', "\\begin{frame}\<CR>\\frametitle{<++>}\<CR><++>\<CR>\\end{frame}<++>",'tex')``
2. In the instance of vim, type ``:source $MYVIMRC`` or restart your vim instance.
3. To activate the environment, type ``EFE``.
