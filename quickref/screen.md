---
layout: post
title: Quick Reference of Screen
mathjax: false
related: false
comments: false
---

_Last modified: Jan, 2010_


## Keybindings

**Ctrl +a (and release), +**

```
symbol | meaning  
?      |  help  
c      |  create  a new screen  
n      |  next screen           
p      |  previous screen       
1      |  switch to screen shell No.1  
"      |  list screen sessions       
d      |  detach a screen  
A      |  rename a screen session 
```

**Ctrl +a** means signal will be sent to "screen" instead of interactive shell. For more info refer to man screen for a
whole list of keybindings


## Commands

```
commands                 |  meaning  
screen -ls               |  show all running screens  
screen -r <session name> |  resume a screen session that was either detached or terminated abnormally 
```


## Screenrc Examples

Create __~/.screenrc__ in which:

```
hardstatus alwayslastline
hardstatus string '%{= kG}[ %{G}%H %{g}][ %{=kw}%?%-Lw%?%{r}(%{W}%n*%f%t%?(%u)%?%{r})%{w}%?%+Lw%?%?%=%{g}][%{B}%Y-%m-%d %{W}%c %{g}]'
defscrollback 1000
startup_message off
```

for basic screen caption. More intro is [here](http://www.softpanorama.org/Utilities/Screen/screenrc_examples.shtml).

Another cheatsheet online is [here](../../assets/files/wiki/screen_cheatsheet.pdf). 

