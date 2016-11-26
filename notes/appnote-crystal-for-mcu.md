---
layout: post
published: true
title: Crystal for MCUs
mathjax: false
related: true
comments: true
categories:
- ee
---

_App Note: [Oscillator design guide for STM8S STM8A and STM32 MCUs (AN2867)](http://www.st.com/web/en/resource/technical/document/application_note/CD00221665.pdf) from ST_

_Time: Dec, 2015_


The app notes assume the design constains passsive parts (oscillator + caps + maybe resistor) and a MCU. 

* To determine whether a certain crystal oscillator is able to oscillate, one needs to do the following. Calculate the crystal loop critical (transconductance) gain of the passive side (crystal + load capacitors) first, and then compare with the oscillator (transconductance) gain from the MCU. The former is the minimal gain needed to sustain the oscillation, and the latter is the gain MCU can provide. The gain margin, the ratio of MCU oscillator gain (latter) over loop critical gain (former), should be at least 5. 

* Use an external resistor (Rext) to limit the drive level (current loading) if necessary. The drive level needs to be measured experimentally. It essentially forms a voltage divider with loading capacitor CL2. So the value of Rext is around the reactance of CL2. After adding Rext, it is better to recalculate the gain margin to ensure it is still large enough. 

* Use guard rings around the crystal circuit to avoid cross talk. Use a separate ground plane for the crystal circuit and connect it to the nearest ground pin on MCU. 

* Concluding remarks: 

>"The most important parameter is the gain margin of the oscillator, which determines if the
oscillator will start up or not. This parameter has to be calculated at the beginning of the
design phase to choose the suitable crystal for the application. The second parameter is the
value of the external load capacitors that have to be selected in accordance with the C L
specification of the crystal (provided by the crystal manufacturer). This determines the
frequency accuracy of the crystal. The third parameter is the value of the external resistor
that is used to limit the drive level. In the 32 kHz oscillator part, however, it is not
recommended to use an external resistor.

>Because of the number of variables involved, in the experimentation phase you should use
components that have exactly the same properties as those that will be used in production.
Likewise, you should work with the same oscillator layout and in the same environment to
avoid unexpected behavior and therefore save time. "