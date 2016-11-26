---
layout: post
title: AVR & Arduino Notes
mathjax: false
related: false
comments: true
published: true
---

_Created: Feb, 2014_

_Last modified: Oct, 2015_



## AVR characteristics

* Max voltage, current and CPU frequency:

![AVR CPU freq 1](http://i.imgur.com/n3TKMUD.png)

![AVR CPU freq 2](http://i.imgur.com/Mxs5Fgc.png)


## AVR registers

* IO stuff: DDRx sets the direction, 0 for input and 1 for output for each bit; PORTx sets the output of each pin, 0 for low and 1 for high for each bit; PINx reads the digital input, 0 for low and 1 for high for each bit. Example:

```
DDRB = (1<<DDB0)|(1<<DDB4)|(1<<DDB5)|(1<<DDB7);
PORTD = (1 << PD0)|(1 << PD3)|(1 << PD6);
if (PINC == 0x01) {...};  // input is usually used in condition statement
```


## Arduino General

* [How to install an Arduino library on Linux](https://learn.adafruit.com/adafruit-all-about-arduino-libraries-install-use/installing-a-library-on-linux)


## Make Arduino low power

Some posts below may help:

* [Use Jeelab library](https://www.openhomeautomation.net/arduino-battery/)
* [An amazing thread discussing the low power tricks on ATMega328](http://gammon.com.au/power)
* [A tutorial from SparfFun](https://www.sparkfun.com/tutorials/309)


## How to program Arduino Pro Mini from Arduino IDE

* Connect VCC(VBUS), GND, TX and RX pins to another USB to serial converter (such as CH340) correctly. 
* Open IDE, "Tools" --> "Board" --> choose "Arduino Pro or Pro mini", and "Tools" --> "Processor" --> choose "Atmega328, 3.3V, 8MHz". 
* When ready to upload code, kee pressing the reset button on Arduino pro mini, and then click "upload" in IDE. Release the button until "uploading" shows up in the IDE. 
* One can use Arduino IDE to write AVR C code directly (don't need to be restricted to setup() and loop() Arduino style functions).
* Haven't been successful in downloading code to AVR using Arduino ISP approach. Use USB-serial adapter to do that for now. 
* The AVR libraries headers can be found at __"C:\Program Files (x86)\Arduino\hardware\tools\avr\avr\include"__.


## Troubleshooting

* Problem: Arduino IDE cannot connect to serial "/dev/ttyACM0" (Arduino Uno)

Solution: After trying various things, it works after I changes the connection from a USB 2 to USB 3 port.


## Links

* [Easy to follow tutorials on MaxEmbedded](http://maxembedded.com/2011/06/port-operations-in-avr/)
* [Arduino build process](https://www.arduino.cc/en/Hacking/BuildProcess)

