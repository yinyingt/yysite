---
layout: post
title: Quick Reference of LyX
mathjax: false
related: false
comments: false
---

__Last modified: Nov, 2011__


## Installation

__Compile LyX in Ubuntu__

First install TeXLive. Due to the insanely out-of-date TeXLive package in Ubuntu repositories, [install TeXLive from source is recommended](http://www.tug.org/texlive/). 

After that, install dependencies: (my OS is Xubuntu 11.10)

```
sudo apt-get install g++ zlib1g-dev libqt4-dev
```


__No spell checker in LyX__

It happened for my self-compiled LyX 2.0.1 on Xubuntu 11.10, where there was no spell checker in "'Tools' -> 'Preference' -> 'Language Settings' -> 'Spellchecker'". The reason was I didn't compile LyX with the spell check libraries. To solve this problem, first install 'libaspell-dev'

```
sudo apt-get install libaspell-dev
```

and recompile LyX. If you need additional dictionaries, install them too before recompilation, such as 'aspell-es' for Spanish. After that it should be good to
go. Open Lyx, and do "'Tools' -> 'Preference' -> 'Language Settings' -> 'Spellchecker'", and check "'Spellcheck continuously'".



## Basic operations

__Insert figure__

http://www.lyx.org/Walkthrough3

__Use BibTeX in LyX__

'Insert' -> 'List/TOC' -> 'BibTeX Bibliography', and add the database file. 

__Insert math in LyX__

http://elyxer.nongnu.org/lyx/Math.html#toc-Subsubsection-18.2.1

Basically, it is 'Insert' -> 'Math' -> 'AMS align Environment'. To add more than one line in an align environment, click the 'add row' icon on the toolbar at the bottom of LyX editor. 

The align environment in LyX adds no line number at the end of each line by default. To add the line number, right click on the line and select 'Number This Line', or press keybinding __Alt +M Shift+N__.

__Insert code in LyX__

To insert code in LaTeX one can simply use 'verbatim' environment. What's more, LyX supports [LaTeX package 'listtings'](http://en.wikibooks.org/wiki/LaTeX/Packages/Listings) to format code snippets with advanced features like syntax hightlighting, numbering lines, and etc.

'Insert' --> 'File' --> 'Child Document', and browse the script file you want to insert, and select 'Program listing' in 'include type'. If I insert a Matlab script, I usually have those parameters in 'More parameters' box (essentially it is 'lstset' environment):

{% highlight latex %}
basicstyle={\footnotesize}
escapeinside={\%*}{*)}
frame=single
language=Matlab
numbers=left
numbersep=5pt
numberstyle={\footnotesize}
showspaces=false
showstringspaces=false
showtabs=false
stepnumber=1
{% endhighlight%}

For more information, look at [a short intro in Wikibooks](http://en.wikibooks.org/wiki/LaTeX/Packages/Listings), or [package 'listings' page in CTAN](http://www.ctan.org/tex-archive/macros/latex/contrib/listings/).

NOTE: LyX seems to have some issues with 'background' option in '\lstset' environment. For example, if I have background={\color{white}} in the 'More parameters' box, there will be compiling errors. 


## Use REVTeX4 with Lyx

__Author and affiliation format__

Sometimes one author is affiliated to more than one institutes. To format grouped addresses which are also superscripted next to corresponding authors:

* Add "'groupedaddress, superscriptaddress'" to revtext4's class option ("'Document' -> 'Setting' -> 'Document class' -> 'class option'").

* Above everything in LyX document, insert raw LaTeX code ("'Ctrl + L'")

{% highlight latex %}
\author{John Smith}
\affiliation{Prime Construction Inc.}
\affiliation{Mr. & Mrs. Smith LLC}

\author{Jane Smith}
\affiliation{Wall Street Networking Solutions}
\affiliation{Mr. & Mrs. Smith LLC}
{% endhighlight %}

If you place this code somewhere else (like below the title), LyX will get confused and insert "'\maketitle'" command in a wrong place, which screws everything up. 

## LyX command line

* Generate pdf files in command line

```
lyx --export pdf2 myfile.lyx
```
