---
layout: post
published: true
title: Schmitt Trigger Astable Multivibrator
mathjax: true
related: true
comments: true
---




_Created: Sep, 2015_

Schmitt trigger inverter can be used to generate square waves as follows (picture taken from [this post](http://www.electronics-tutorials.ws/waveforms/generators.html)):

![Schmitt trigger inverter astable multivibrator](http://www.electronics-tutorials.ws/waveforms/tim27.gif?81223b)

The question is what is the frequency of this astable multivibrator? 

Assume we have a Schmitt trigger inverter with output high voltage of V\_OH, output low voltage of V\_OL, rising edge threshold voltage of V\_T+, and falling edge threshold voltage of V\_T-. In the steady state, one cycle of oscillation goes as follows: 

1. Initial condition: V\_out is at V\_OL, and then input voltage V\_in falls to V\_T- 
2. V\_out becomes V\_OH, which charges the input capacitor towards V\_T+
3. V\_in rises to V\_T+ again, making V\_out switch to V\_OL again

From State 1 to 2, input voltage V\_in can be written as 

<div>
\[ V_{in} = V_{OH} - (V_{OH} - V_{T-}) \exp(-\frac{t}{\tau}) \]
</div>

This process ends when V\_in reaches V\_T+. The duration T_1 can be got by solving 

<div>
\[ V_{T+} = V_{OH} - (V_{OH} - V_{T-}) \exp(-\frac{T_1}{\tau}) \]
</div>

yielding

<div>
\[ T_1 = \tau \ln \frac{V_{OH} - V_{T-}}{V_{OH} - V_{T+}}\]
</div>

From State 2 to 3, input V\_in is 

<div>
\[ V_{in} = V_{OL} + (V_{T+} - V_{OL}) \exp(-\frac{t}{\tau}) \]
</div>

The process ends when V\_in becomes V\_T-. The duration T\_2 can be solved from

<div>
\[ V_{T-} = V_{OL} + (V_{T+} - V_{OL}) \exp(-\frac{t}{\tau}) \]
</div>

leading to 

<div>
\[ T_2 = \tau \ln \frac{V_{T+} - V_{OL}}{V_{T-} - V_{OL}} \]
</div>

It is easy to see that the generated square wave can be asymmetric, and the period is 

<div>
\[ T = T_1 + T_2 = \tau (\ln \frac{V_{OH} - V_{T-}}{V_{OH} - V_{T+}} + \ln \frac{V_{T+} - V_{OL}}{V_{T-} - V_{OL}}) \]
</div>
