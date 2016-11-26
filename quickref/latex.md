---
layout: post
title: Quick Reference of LaTeX
mathjax: false
related: false
comments: false
---

_Last modified: Feb, 2014_


## TeXLive

I use TeXLive for anything related to LaTeX in all my Linux machines. Installing from the source rather than distribution's repositories is actually preferred. 

__Get TeXLive__

I use rsync to grab the TeXLive source. The advantage is I can easily update the source to the latest version with rsync. Here is the rsync command:

```
rsync -a --delete --progress  rsync://mirror.unl.edu/ctan/systems/texlive/tlnet/ $HOME/texlive_src
```

Refer to [this link](http://www.tug.org/texlive/acquire-mirror.html) for further information and [this link](http://ctan.org/mirrors) for more mirrors. 


__Install TeXLive__

Easy:

```
cd $HOME/texlive_src
perl install-tl
```

Just follow the instructions after that. 


__Install and update packages__

The program 'tlmgr' helps you with that:

```
tlmgr search [keyword]
tlmgr install [package_name]
```

To update TeXLive: 

```
tlmgr update --self
tlmgr update --all
```


## Install a LaTeX package

Sometimes when you compile a LaTeX source, there may be error messages like

```
! LaTeX Error: File `esint.sty' not found.
```

It means the style file "esint" is not found in the system. This can be resolved by installing package 'esint'.


__Method 1: use repository__

In TeXLive, command __tlmgr__ is used to manage TeX packages. 

```
tlmgr install esint
```

A somewhat detailed usage of tlmgr can be found at [tlmgr@TeXLive](http://www.tug.org/texlive/doc/tlmgr.html), or simply 

```
man tlmgr
```


__Method 2: install from source__

Just download 'esint' from [CTAN](http://www.ctan.org/pkg/esint), unzip to a local directory like /home/user/tex/, and run command 

```
texhash home/user/tex
```

Or copy the downloaded package to "texlive/201X/texmf-local/tex/latex/local", and do 

```
texhash
```


## A basic Beamer template 

{% highlight latex %}
 \documentclass{beamer}     
    \mode<presentation>
    {
      \usetheme{Berkeley}
    % check out http://www.pletscher.org/writings/latex/beamerthemes.php for more Beamer themes
      \setbeamercovered{transparent}
      % or whatever (possibly just delete it)
    }
     
    \usepackage[english]{babel}
     
    %\usepackage[latin1]{inputenc}
    % not needed if in English
     
    %\usepackage{times}
    %\usepackage[T1]{fontenc}
    % Or whatever. Note that the encoding and the font should match. If T1
    % does not look nice, try deleting the line with the fontenc.
     
    \title[title's short name]
    {The Status of My Project}
    \subtitle
    {} % (optional)
     
    \author{Lijun}
     
    \institute{
    The University of Nowhere}
     
    \begin{document}
     
    \begin{frame}
      \titlepage
    \end{frame}
     
    %\begin{frame}{Outline}
    %  \tableofcontents
    %\end{frame}
     
    \section[s1]{Section 1}
     
    \begin{frame}{Title of frame 1}
    \makebox{\includegraphics[width=45mm]{cmos_dc.png}} 
     \begin{itemize}
       \item
       Item 1
       \item
       Item 2
     \end{itemize}
    \end{frame}
     
    \section[s1]{Section 2}
     
    \begin{frame}{Frame 2}
     
    \begin{columns}[c]
    \column{1.8in} \\
    \makebox{\includegraphics[height=60mm]{b1.png}} \\
     
    \column{1.8in} \\
    \makebox{\includegraphics[height=60mm]{b2.png}} \\
     
    \end{columns}
    \end{frame}
     
    \end{document}
{% endhighlight %}


## Logos in Beamer

Beamer has "\logo" command, but I use package "'textpos'". Basically this package give users more control over the position of a "box" in LaTex text.

First enable this package: 

{% highlight latex %}
\usepackage[absolute, overlay]{textpos}
{% endhighlight %}

Here "overlay" makes logos appear above Beamer's text layer. Then define a macro that inserts logos

{% highlight latex %}
\setlength{\TPHorizModule}{10mm} % horizontal unit length
\setlength{\TPVertModule}{\TPHorizModule} % vertical unit length
\textblockorigin{1mm}{1mm} % define origin at the upper left corner

\newcommand{\mylogo}{
\begin{textblock}{1}(11,0.2)  % numbers here are in terms of unit length defined above
    \includegraphics[height=10mm]{mylogo.png}
\end{textblock}
}
{% endhighlight %}

This draw a logo at the upper right corner. The code is self-explanatory.

TIPS: Do not use "_", "-", "." and so on for macro names, or errors occur.

Insert command "\mylogo" in pages that need the logo.


__Reference__

* [Textpos @ctan.org](http://www.ctan.org/tex-archive/macros/latex/contrib/textpos) and its [manual](http://mirrors.ctan.org/macros/latex/contrib/textpos/textpos.pdf)
* [A brief textpos intro](http://handyfloss.net/2007.05/latex-the-textpos-package/)


## BibTeX

__BibTeX entry format__

A BibTeX entry example is like this 

```
@ARTICLE{sc_eff_tables_v37,
  author = {Martin A. Green and Keith Emery and Yoshihiro Hishikawa and Wilhelm
  Warta},
  title = {Solar Cell Efficiency Tables (Version 37)},
  journal = {Prog. Photovolt: Res. Appl.},
  year = {2011},
  volume = {19},
  pages = {84}
}
```

Note that 

* In author field, one should specify names like "A and B and C and D" instead of "A, B, C and D". The former will be automatically formatted to "A, B, C and D" in Tex compilation (and appear like that afterwards), while the later will give errors in compilation. 

* All characters except the first one in "title" field will be decapitalized after being processed by BibTeX. To keep the default style, one can encapsulate the part with \{\}. One can also use LaTeX macros within \{\} in these fields too. For example, 

{% raw %}
>@MISC{example1,
>   author = {A and B and C},
>   note = {{\LaTeX} in {X}},
>   year = {2012}
>   }
{% endraw %}

* To type math expressions in BibTeX, use "$...$" environment. 

* To insert URL in BibTeX entry, don't forget including

{% highlight latex %}
\usepackage{url}
{% endhighlight %}

in the LaTeX preamble. Otherwise there will be compiling error. 

__Reference__

* [Basic BibTeX](http://www.cs.arizona.edu/~collberg/Teaching/07.231/BibTeX/bibtex.html)
* [BibTeX and bibliography styles](http://amath.colorado.edu/documentation/LaTeX/reference/faq/bibstyles.html)


__BibTeX citation order__

Question: By default BibTeX orders citations alphabetically (by the last name of first author) in "plain" bibliography style. How to order citations by order of appearance? 

Answer: Use 'unsrt' (unsorted?) bibliography style. For example, 

{% highlight latex %}
\bibliographystyle{unsrt}
\bibliography{mybib}
{% endhighlight %}

__Reference__

* [Order citation by appearance in bibtex](http://stackoverflow.com/questions/144639/how-do-i-order-citations-by-appearance-using-bibtex)


## Tables

* Use package *hhline* to extend the "\hline" functionalities. Enable it by adding 

{% highlight latex %}
\usepackage{hhline}
{% endhighlight %}

in the preamble. Package website is http://www.ctan.org/pkg/hhline. 

* Use \cline to draw row line across specific columns. For example:

{% highlight latex %}
\begin{tabular}{|c|c|c|}
aaa & bbb & ccc \\ \cline{1-2}
ddd & eee & fff \\ \hline
\end{tabular}
{% endhighlight %}

* Change background color in table cells: use package 'colortbl'. 

{% highlight latex %}
\usepackage{colortbl}
\begin{tabular}{|c|c|c|}
\rowcolor{yellow} aaa & bbb & ccc \\ \cline{1-2}
ddd & eee & fff \\ \hline
\end{tabular}
{% endhighlight %}

More information can be found at [CTAN repo](http://www.ctan.org/tex-archive/macros/latex/contrib/colortbl). 


## Fonts

__Fontsize__

* [A chart from wikibook](http://en.wikibooks.org/wiki/LaTeX/Formatting#Sizing_text)

## Symbols

* To write "V~IR" in latex: 

```
V \sim IR
```

## Math Equations

* To supress the numbering for certain lines between "'align'" environment, use 
```
\notag 
```

For example,

{% highlight latex %}
\begin{align}
S& = 1 + 1 \notag \\
& = 2
\end{align}
{% endhighlight %}

* To write matrices and other arrays 

http://www.maths.tcd.ie/~dwilkins/LaTeXPrimer/Matrices.html


__Common Confusion__

* Brackets that matches the text size inside:

{% highlight latex %}
\left( xxxxx \right)
\left[ xxxxx \right]
\left\{ xxxxx \right\}
{% endhighlight %}

## TeXMaker

TeXMaker is an excellent TeX IDE I used. However, in my 64-bit machine, I have to do a few hacks to make it work. 

```
sudo ln -s /usr/lib/x86_64-linux-gnu/libtiff.so.4 /usr/lib/libtiff.so.3
sudo ln -sf /usr/lib/x86_64-linux-gnu/libjpeg.so.8 /usr/lib/libjpeg.so.62
```

## Miscellaneous Commands

__Insert code blocks in LaTeX__ 

The easiest way is to put them in 'verbatim' environment

{% highlight latex %}
\begin{verbatim}
#!/usr/local/env python

import numpy as np 
import matplotlib.pylab as plt

x = np.linspace(0,6,100)
plt.plot(x, np.sin(x))

\end{verbatim}
{% endhighlight %}

* Insert a horizontal line with '\line' command. Syntax: 

{% highlight latex %}
\line(direction_x, direction_y){length}
{% endhighlight %}

For example: 

{% highlight latex %}
\begin{center}
\line(1, 0){200}
\end{end}
{% endhighlight %}

draws a line with direction (1,0) and length of 200. 

* Insert a new line in a table cell

First define a "new command" like: 

{% highlight latex %}
\newcommand{\specialcell}[2][c]{
  \begin{tabular}[#1]{@{}c@{}}#2
\end{tabular}}
{% endhighlight %}

in preamble. To use it: 

{% highlight latex %}
Foo bar & \specialcell{Foo\\bar} & Foo bar \\    % vertically centered
Foo bar & \specialcell[t]{Foo\\bar} & Foo bar \\ % aligned with top rule
Foo bar & \specialcell[b]{Foo\\bar} & Foo bar \\ % aligned with bottom rule
{% endhighlight %}

Refer to [this post](http://tex.stackexchange.com/questions/2441/how-to-add-a-forced-line-break-inside-a-table-cell) for details. 
