---
layout: post
title: Use Circuitikz and Publish
mathjax: false
related: false
comments: true
---

_First created: Aug, 2013_

I find circuitikz is a quite neat LaTeX package for circuit drawing. 

Example code: 

{% highlight latex %}
% This is a simple RC circuit
\documentclass{standalone}
\usepackage[siunitx]{circuitikz}
\usepackage{tikz}


\begin{document}
\begin{circuitikz} \draw
(0,0) to [american voltage source] (0,2) to [R, l=$1\Omega$] (2,2) to [C, l=1F] (2,0) -- (0,0)
(0,0) node [ground]{} 
;
\end{circuitikz}

\end{document}
{% endhighlight %}

What if I want to export the circuit to another format other than pdf or ps?  

* Convert it to svg file, there is no direct way. One solution is:  

{% highlight bash %}
latex RC_circuit.tex   # to dvi
dvips RC_circuit.dvi   # to ps 
inkscape RC_circuit.ps --export-plain-svg=RC_circuit.svg
{% endhighlight %}
<br>

* Convert it to png:

{% highlight bash %}
latex RC_circuit.tex   # to dvi
dvips RC_circuit.dvi   # to ps 
inkscape -z --export-png=simple_RC_circuit.png -w 1024 -h 1024 simple_RC_circuit.ps
{% endhighlight %}
