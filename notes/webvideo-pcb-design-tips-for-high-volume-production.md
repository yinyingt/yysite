---
layout: post
published: true
title: PCB Design Tips for High Volume Production
mathjax: false
related: true
comments: true
categories:
- ee
---

_Time: Jul, 2014_

_Presenter: [David Jones](http://www.eevblog.com/)_


For those who made PCB in small quantities like me, I found the following Youtube video made by [David Jones](http://www.eevblog.com/about/) (the entertaining engineer hosting [EEVblog](http://www.eevblog.com/) and [The Amp Hour](http://www.theamphour.com/)) both interesting and eye-opening: 

<div>
<iframe width="560" height="315" src="//www.youtube.com/embed/VXE_dh38HjU" frameborder="0" allowfullscreen></iframe>
</div>


The video ran about 50 minutes, but was worth the time since it covered lots of useful tips with vivid demos. 

The notes below summarize the major tips in the video, which may be useful for those who want to get it quick or simply don't have the time: 

1. Convert through hole parts to surface mount if possible. Modern PCB assembly lines are much more friendly to surface mount parts. Generally surface mount parts are quicker and less expensive to be put on boards.
2. Use widely used and easily available parts. The reason is self-explanatory. 
3. Buy parts (eg resistors, capacitors) in reels, not in cut tapes. Buy ICs in tubes, not in loose packs. Reels and tubes can be mounted to the PCB assembly lines easily, while cut tapes and loose packs need extra efforts thus extra charges. 
4. Consolidate the components. A diversified component portfolio costs big money. For example, it can be eliminating the sparse use of 20 kOhm resistors by using two 10 kOhm resistors in series for each. 
5. Design to manufacture: make the boards manufacturable (e.g., paying attention to the solder mask, choosing the proper pad size, etc.).
6. Design to test: make the boards easy to test (e.g., make the microcontrollers easy to be programmed in-circuit).
7. Use spreadsheet to manage bom. It can be really beneficial in long run.
8. Panelize boards, but also check the size compatibility between PCB manufacturer and board assembler.
9. Design how to snap off boards from a panel, using v grooving or routing or both.
10. It is important to put [fiducial marks](http://www.ladyada.net/library/pcb/fiducials.html) on board for the assembling machine to calibrate. It is not only necessary for big panels, but also local components if it has dense features (eg. BGA package). 
11. Use gold plating for high pin count packages such as BGA, because gold tends to have flat surface. 
12. Talk to PCB assembler about the assembly requirements before designing panels. 
