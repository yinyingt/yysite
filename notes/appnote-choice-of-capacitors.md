---
layout: post
published: true
title: Choice of Capacitors
mathjax: false
related: true
comments: true
categories:
- ee
---


_Time: Oct, 2013_


Circuit parts like resistors and capacitors can be made of different materials and different technologies. Choosing the right ones for specific application seems like black magic sometimes. But there are some general rules. 

* First of all, the host of [EEVblog](http://www.eevblog.com/) has made two very good videos tutorials ([part1](http://www.youtube.com/watch?v=xlvqUts9H9c), [part2](http://www.youtube.com/watch?v=TDDoi70cxw0)) on choosing capacitors. 

* [Capacitor selection chart](http://www.phys.ufl.edu/~eshop/caps/caps.html) gives a brief and very useful summary. 

* From pages 8.112~113 in Ref 1, there are two comparison charts for resistors and compacitors

![Resistor comparison chart]({{ site.baseurl }}/assets/files/wiki/img7_resistor_comparison_chart.png)

![Capacitor comparison chart]({{ site.baseurl }}/assets/files/wiki/img8_capacitor_comparison_chart.png)

* From page 18 of Ref 2, and I quote

<blockquote>
<strong>Capacitors</strong> <br />

– Avoid values less than 100 pF. <br />

– Use NPO if at all possible. X7R is OK in a pinch. Avoid Z5U and other low quality dielectrics. In critical applications, even higher quality dielectrics like polyester, polycarbonate, mylar, etc., may be required. <br />

– Use 1% tolerance components. 1%, 50V, NPO, SMD, ceramic caps in standard E12 series values are available from various sources. <br />

– Surface mount is preferred. <br />

<strong>Resistors </strong><br />

– Values in the range of a few hundred ohms to a few thousand ohms are best. <br />

– Use metal film with low temperature coefficients. <br />

– Use 1% tolerance (or better). <br />

– Surface mount is preferred. <br />
</blockquote>

<br />

## Reference

* Ref 1: [Basic Linear Design, Chaper 8 Analog Filters, from  Analog Devices](http://www.analog.com/library/analogdialogue/archives/43-09/edch%208%20filter.pdf)

* Ref 2: [Active Low-Pass Filter Design, from Texas Instruments](http://www.ti.com/litv/pdf/sloa049b)
