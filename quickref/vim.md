---
layout: post
title: Vim Notes
mathjax: false
related: false
comments: true
published: true
---


_Created: Jan, 2012_

_Last modified: Feb, 2016_


A tiny subset of VIM commands I find very useful...

## Windows

* Split window vertically: "C-w v"
* Split window horizontally: "C-w s"
* Switch among windows: "C-w w"
* Resize the current window horizontally: ":resize 60" (resize to 60 rows); ":resize +5" (increase by 5 columns); can also use "res" instead of "resize" for abbreviation. 
* Resize the current window vertically: everything is the same to horizontal resizing by replacing "resize" with "vertical resize".
* [Use file explorer netrw](http://vimcasts.org/episodes/the-file-explorer/)


## General

* Insert characters in multiple lines (such as inserting space at the head of multiple lines): see [this post](http://vim.wikia.com/wiki/Inserting_text_in_multiple_lines)

* How to [use Vundle to manage VIM plugins](https://www.digitalocean.com/community/tutorials/how-to-use-vundle-to-manage-vim-plugins-on-a-linux-vps). 

* [Open multiple files in new tabs or split windows and navigate](http://askubuntu.com/a/537972): this can be handy when working on multiple files. 

* Go to line: (at least) 3 different ways
  - `42G`
  - `42gg`
  - `:42<CR>`

* Move forward by one word: `w`; move forward by 3 words: `3w`.
* Move backward by one word: `b`; move backward by 3 words: `3b`.
* Move to the beginning of the line: `0`; move to the non-blank character of the line: '^'.
* Move forward by one sentence: `)`; move backward by one sentence: `(`.
* Move forward by one paragraph: `}`; move backward by one paragraph: `{`.
* Move to the top of screen: `H`; middle of screen: `M`; and bottom of screen: `L`

* Undo: `u`; redo: `Ctrl+r`.


## Using marks

[This page](http://vim.wikia.com/wiki/Using_marks) summarizes it well. 

## Indentation Settings

Take care of indentation of C/C++ code ([this link](http://stackoverflow.com/questions/97694/auto-indent-spaces-with-c-in-vim)), and generally [the indentation in Vim](http://vim.wikia.com/wiki/Indenting_source_code).

Put the following lines to "~/.vimrc":

```
set autoindent
set shiftwidth=4
set softtabstop=4
set expandtab
```

Here "autoindent" does no more than indenting following the previous line. For C/C++ files one may explore "cindent" option. 

To disable autoindentation when pasting text, do this in normal mode:

```
:set paste
```

After done with pasting, do this to re-enable autoindentation with 

```
:set nopaste
```

Another trick is to set a key shortcut to toggle this "paste action" by adding this line to '~/.vimrc': 

```
set pastetoggle=<F10>
```

which will associate key F10 with this toggling behavior.  

Reference [here](http://stackoverflow.com/q/2514445).


## Color

By default, the comments are lightlighted with dark blue in a black background, which is hard to see in a terminal. I find using "desert" color theme in a black ground works better. Add this line to "~/.vimrc":  

```
colors desert
```

See some discussions [here](http://unix.stackexchange.com/questions/88879/vim-better-colors-so-comments-arent-dark-blue) and [here](http://vim.wikia.com/wiki/Change_the_color_scheme).

## Search & Replace

* [Search and replace in normal text mode](http://vim.wikia.com/wiki/Search_and_replace). Some commonly used commands

To search and replace A with B in the current line

```
:s/A/B/g
```

To search and replace A with B globally (all lines)

```
:%s/A/B/g
```

To search and replace A with B globally with confirmation of each match

```
:%s/A/B/gc
```

* [Search and replace in visual selection_mode](http://vim.wikia.com/wiki/Search_and_replace_in_a_visual_selection)


## Command-line interaction

* To run a shell command with the file being edited like

```
:! g++ %
```

which will compile the current file with GCC C++ compiler. 

* To run a command in certain working directory, first check the working directory with 

```
:pwd
```

To change the working directory to the directory the file being editing is in, run

```
:cd %:p:h
```

Explanation of the above command: % gives the name of the current file, %:p gives its full path, and %:p:h gives its directory (the "head" of the full path).

For more information on this topic, see [this post](http://vim.wikia.com/wiki/Set_working_directory_to_the_current_file).


## Visual block mode

Press `Ctrl+v` to enter visual block mode. It is amazing. See [this tutorial](http://vimcasts.org/episodes/selecting-columns-with-visual-block-mode/) for a brief introduction. 
