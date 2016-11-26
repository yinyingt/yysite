---
layout: post
title: Magnitude and Frequency Scaling
mathjax: true
related: false
comments: true
---

_Last modified: Oct, 2013_

Magnitude and frequency scaling is very useful when designing filters. Given an existing design (e.g., a low-pass filter with certain low-cutoff frequency, and of course, the values of resistor, capacitor and inductor), one can use scaling and quickly calculate the values of R, C and L for another similar design but with different cut-off frequency. 

The scaling relationships are: 

<div>
\[R^{\prime} = s_m R\]
</div>

<div>
\[L^{\prime} = \frac{s_m}{s_f} L\]
</div>

<div>
\[ C^{\prime} = \frac{C}{s_m s_f} \]
</div>

where R, L, C are the initial values, R', L' and C' are the scaled values, <span>\\( s_m \\)</span> is the magnitude scaling factor, and sf is the frequency scaling factor.

How does it work? A general procedure is first set cut-off frequency \\( \omega_0 \\) to 1 rad/s, and set some component values to easy numbers, such as 1F or 1 Ohm. Calculate the other component values based on that. Finally, scale everything to the ones you want. 

There are two examples below: 

## Example 1: simple RC filter

Consider a simple RC low-pass filter like this: 

![simple RC circuit]({{site.baseurl}}/assets/files/wiki/img1_simple_RC_circuit.png)

This is a low-pass filter with 3dB bandwidth of 

<div>
\[ \omega_{3dB} = \frac{1}{RC} = 1 \text{rad/s}\] 
</div>

or

<div>
\[ f_{3dB} = \frac{1}{2 \pi RC} = \frac{1}{2 \pi} \text{Hz} \]
</div>

Now, I need to design a similar filter with 3dB bandwidth of 1kHz, and I want to use a 1nF capacitor instead. The question is: what resistor value should I use? 
First I calculate the frequency scaling factor: 

<div>
\[ s_f = \frac{1000Hz}{f_{3dB}} = 2000\pi \]
</div>

and then calculate the magnitude scaling factor

<div>
\[ s_m = \frac{C}{s_f C^{\prime}} = \frac{1}{2000\pi 10^{-9}} = 1.59\times 10^5 \]
</div>

At last, the new resistor value should be 

<div>
\[ R^{\prime} = s_m R = 1.59 \times 10^5 \Omega \]
</div>


## Example 2: Sallen-Key active filter

The previous example is just a very simple example with lumped elements. The simple principle can be used in more complex cases, such as [Salley-Key active filters]({{site.baseurl}}/wiki/analog-filters-and-salley-key-topology.html). Take the 2nd-order high-pass Butterworth filter for example as below: 

![Sallen-Key high-pass]({{site.baseurl}}/assets/files/wiki/img6_sallen_key_high_pass.png)

with transfer function 

<div>
\[ H(s) = \frac{H_0 s^2}{s^2 + \frac{\omega_0}{Q} s + \omega_0^2 } \]
</div>

where 

<div>
\[ \omega_0 =\frac{1}{\sqrt{R_1 R2 C_1 C_2}} \]
</div>

and 

<div>
\[ Q =  \frac{\sqrt{R_1 R2 C_1 C_2}}{R_2 (C_1 + C_2) + R_1 C_2 (1-H_0)} \]
</div>

For the convenience, first set the cut-off frequency 

<div>
\[ \omega_0 = 1 \text{rad/s} \]
</div>

and both capacitors C1 and C2 to 1F. Then the Q is reduced to 

<div>
\[ Q =  \frac{\sqrt{R_1 R_2 }}{(1-H_0) R_1 + 2 R_2 } \]
</div>

Since Q= \\( \frac{\sqrt{2}}{2} \\) for Butterworth filter, we can solve R1 and R2 from the equations:

<div>
\[ \frac{1}{\sqrt{R_1 R2 }} = 1 \]
\[ \frac{\sqrt{R_1 R2 }}{(1-H_0) R_1 + 2 R_2 } = \frac{\sqrt{2}}{2} \]
</div>

The solutions can be easily got like this: 

<div>
\[ R_2 = \frac{\sqrt{2} + \sqrt{8 H_0 -6}}{4} \]
\[ R_1 = \frac{4}{\sqrt{2} + \sqrt{8 H_0 -6}} \]
</div>

Now, if I want to design a Sallen-Key unity-gain (H0=1) Butterworth filter with cut-off frequency of 1kHz, using capacitors 1uF, what are the values of resistors? This can be easily done with scaling techniques as follows. 

First find the frequency scaling factor: 

<div>
\[ s_f = \frac{1 \text{kHz}}{\frac{1}{2 \pi} \text{Hz}} = 2000 \pi\]
</div>

Then calculate the magnitude scaling factor: 

<div>
\[ s_m = \frac{1 \text{F}}{s_f \times 1 \mu\text{F}} = \frac{500}{\pi}\]
</div>

Therefore, the two resistor values are: 

<div>
\[ R_2 = \frac{500\sqrt{2}}{2 \pi} \approx 112.5 \Omega \]
\[ R_1 =  \frac{500\sqrt{2}}{ \pi} \approx 225 \Omega \]
</div>

