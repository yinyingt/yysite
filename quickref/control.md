---
layout: post
title: Control Notes
mathjax: true
related: false
comments: false
published: true
---

__Last modified: Jun, 2014__

## PID

The most widely used controller is probably the [proportional-integral-derivative](http://en.wikipedia.org/wiki/PID_controller) (PID) controller. The system can be understood with the transfer function 

<div>
$$G(s) = \frac{1}{as^2 + bs + c}$$
</div>

The overall properties of P, I and D can be summarized as follows: 

|                       |      P                     | I              |    D        |
| --------------------- |-------------               | -----          | ----------- |
| tracking error        | non-zero, but can be close | zero           | no tracking |
| disturbance rejection | non-zero, but can be close | zero           | no          |
| stability             | always stable for Kp>0     | can be unstable| always stable for Kd>0 |

So I is better for tracking and disturbance rejection, while P and D are more for stability. 


