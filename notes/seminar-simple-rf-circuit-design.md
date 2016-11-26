---
layout: post
title: Simple RF Circuit Design
mathjax: false
related: false
comments: true
---

_Time: Mar, 2016_

_Talker: [Michael Ossmann](https://greatscottgadgets.com/)_

<div>
<iframe width="560" height="315" src="https://www.youtube.com/embed/TnRn3Kn_aXg" frameborder="0" allowfullscreen></iframe>
</div>

<br>

Michael is a well-known RF engineer and educator,  famous for his popular [HackerRF One](https://greatscottgadgets.com/hackrf/) SDR kit. He shares his insights of designing practical RF circuits that don't require a PhD to understand. The goal is to make it easy for people to design RF (black magic) circuits with a few very simple guidelines. 


## Rule 1: Use Four Layers

* The typical 4-layer PCB stack-up RF layer -> ground plane -> power plane -> signal layer works well for most RF circuits. 

* Even though 2-layer design seems sufficient sometimes, it is just easier to do 4-layer design which doesn't cost much more (from PCB house such as OSH Park) but is less error-prone. 

* Don't break ground plane and power plane for signal routing. Use more layers (e.g., 6-layer stack) instead. 


## Rule 2: Use Integrated Components

* Nowadays various integrated RF components with quality metrics are readily available through vendors and distributors. Examples are amplifiers, filters (Michael likes SAW filters a lot), switches, transceivers (something like nRF24L01), impedance transformers (e.g., balun), connectors, etc.

* Michael's favorite filter design tool is Digikey's parameter search page :)

* Those RF ICs reduce the learning curve and complexity of building application circuits, and make the design more robust. Also, they often save board area. 

* Therefore, use those monolithic RF ICs as often as possible. 


## Rule 3: Design for 50 Ohms Everywhere

* 50-Ohm design is almost the norm, so make all RF interfaces 50-Ohm compatibile. 

* This involves impedance calculating tools to calculate the microstrip width, but those tools are easy to find on the Internet. 

* Pay attention to the PCB dielectric and stack spacing when changing PCB fab house. Those may have an impact on the impedance. 


## Rule 4: Follow Manufacturer Recommendations

* For components that don't have 50 Ohm interface (such as antenna pinouts on a transceiver IC), use the reference design from the component manufacturer. Do the impedance matching as close to the component as possible. 

* Sometimes, it is even better: you can find a RF IC to match the impedance instead of using standalone inductors and capacitors. For example, Michael is able to find a balun IC to match the antenna interface for nRF24L01+ transceiver. 


## Rule 5: Route RF First

* The reason is straightforward: the RF part of circuits are sensitive to handle and easy to interfere with others. 


## Examples

Michael demonstrates 3 examples following the above 5 rules. He uses RF ICs as many as possible. Watch the video above. 


## Extra Questions/Comments

* Q: How much attention did you pay to the power rating of components?

A: Most components including transceivers are pretty low power those days. But watch out for amplifiers which can draw lots of power. When using amplifiers, be aware of its linearities (1dB power compression point, 3rd order intercept point). 

* Q: FCC approval?

A: If the devices are sellable, FCC certification (equipment authorization to be specific) is a must. However, if the devices are test equipments, FCC certification can be exempted. Be sure to follow manufacturer's rules in design to pass FCC certification. 

* Q: How to decide among PCB antenna, chip antenna, and external antenna? 

A: For PCB antenna and external antenna, it is low cost (PCB) VS performance (external), and low cost (PCB) VS flexibility (external). In terms of PCB antenna and chip antenna, the latter gives better performance per board area (more compact). 
