---
layout: post
title: "Build A Low-Cost Remote Control Vehicle"
mathjax: false
related: false
comments: true
published: true
---



_Created: July, 2015_;
_Last modified: July, 2015_

__Writing in progress...__

In this post, I show how to build a remote control vehicle that is wirelessly controlled via Bluetooth Low Energy. The majority part (vehicle assembly and microcontroller firmware writing) is the result of a whole day long hacking in the 4th of July holiday, 2015. 


## Preparations

The parts below, being low-cost and reasonably of good quality, take about 10 to 14 days to ship from China to United States. 

* 2-wd robot vehicle kit from AliExpress such as [this one](http://www.aliexpress.com/item/High-Quality-2wd-Motor-Smart-Robot-Car-Chassis-Kit-Speed-Encoder-Battery-Box-For-Arduino/32347607537.html): This kit includes a chassis, a battery case, 2 DC motors and some other necessary accessories (tires, screws, etc.). 

* Motor driver: can be as simple as ULN2003 (a simple current driver), or an H-bridge driver such as L293D, either of which can be obtained from AliExpress or eBay as a standalone IC or a module.  

* nRF51822 BLE module, such as [this one](http://www.aliexpress.com/item/NRF51822-Bluetooth-Module-Networking-Module-Wireless-Communication-Module/32281199327.html). I find Nordic's nRF51822 modules are easier to find, and have bigger module-level portfolio. 

* A BLE central: most recent computers and phones already have built-in BLE centals; if not, I recommend CSR 4.0 BLE USB dongle which can be got from Amazon, eBay, AliExpress, etc. 

* Batteries. 

Assembly of robot vehicle kit is very straightforward by using the sample picture on seller's page as a reference. By the end, solder four leads to the contacts of two DC motors. A small breadboard may be helpful. 


## Design & Implementation

The nRF51822 is used as a BLE peripherial, which gets messages from the BLE central and drive two DC motors. PWM is used together with SoftDevice 8.0.0. The duty cycle of PWM is related to the driving current, which is further related to the motor speed. If two motors run at the same speed, the vehicle goes straight forward. If left motor runs faster than the right, the vehicle turns right. If the left motor runs slower than the right, the vehicle turns left. If an H-bridge driver is used, the vehicle can go backward too. 

The RCV is designed to run in two different modes: preprogrammed or live modes. In preprogrammed mode, the vehicle follows a predefined route. In live mode, the vehicle get the real-time message from BLE central and change route accordingly. 

While I am sure there are better implementations, below is a naive implementation that works for me. 

A service with two characteristics, a mode command characteristic and a motion command characteristic, is added to the BLE peripheral. The mode command characteristic accepts commands like start, stop, and pause. The motion command characteristic accepts motion control instructions. Each time the motion command characteristic is written, the instruction is pushed to a queue. A preprogrammed route can be contructed by writing a sequence of motion instructions to motion command characteristic, and gets executed by issuing start command to mode command characteristic. On the other hand, in live mode, the queue size is set to one, and each motion instruction written to motion command characteristic will be executed immediately. 

The data format of motion instructions to be written the motion command characteristic is as follows (from LSB to MSB): 

* properties (1 byte): information like directions
* duration (2 bytes): a 16-bit unsigned int; minimum step is 10 ms. 
* left motor speed (1 byte): between 0 ~ 255
* right motor speed (1 byte): between 0~ 255

Since nRF51 SDK 8.0.0 has no library support for PWM, I used the PWM code [here](https://github.com/lijunhw/nRF51_lib/tree/master/nrf_pwm) which works pretty well with SoftDevice. 

The complete BLE peripheral source code has been released [here](https://github.com/lijunhw/LLRCV). 

For the very first version, I use a basic and popular [ULN2003A motor driver](http://www.ti.com/product/uln2003a) to drive the DC motors because this is the only motor driver IC near my hands. Because ULN2003A is not an H-bridge driver, the RCV cannot go backwards and the first "properties" byte in the motion instruction above is simply ignored. 

On the BLE central side, I use a Python module called [pygatt](https://github.com/ampledata/pygatt) to communicate with RCV. You have to install the latest [BlueZ](http://www.bluez.org/) drivers first because pygatt is simply a wrapper around "gatttools" which is compiled from BlueZ. See my [other post]({{ site.wikiurl }}/linux-bluetooth.html) for installing BlueZ in Linux. The Python code using pygatt can be found [here](https://github.com/lijunhw/LLRCV/tree/master/RCV1_BLE_central/pygatt), which works on my Linux. Of course, you can always use other applications on different platforms, as long as you know how to talk to the BLE peripheral. 


## Action!

Some pictures/videos here.
