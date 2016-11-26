---
layout: post
title: Use USB-to-Serial Adapter in Linux
mathjax: false
related: false
comments: true
---

_Last modified: Nov, 2013_

This is a brief tutorial on how to use a [Startech serial-to-USB adapter](http://www.staples.com/office/supplies/StaplesProductDisplay?storeId=10001&catalogIdentifier=2&partNumber=IM1DE4073) in Ubuntu. The procedure can be extended to other similar products. 

First, find the product ID and vendor ID with the command

```
lsusb
```
For example, the line below

```
Bus 005 Device 002: ID 067b:2303 Prolific Technology, Inc. PL2303 Serial Port
```

means the vendor ID is 067b, and product ID is 2303. 

To use the device in userspace, create a udev rule such as '/etc/udev/rules.d/90-usbserial.rules' with the content like 

```
SYSFS{idVendor}=="067b", SYSFS{idProduct}=="2303", RUN+="/sbin/modprobe usbserial vendor=0x067b product=0x2303", OPTIONS+="last_rule"
```

And then restart the udev daemon:

```
sudo restart udev
```

The following is an example of using Python to write to the serial port:

{% highlight python %}
#!/usr/bin/env python
"""
Usage: python rs232_write.py "*RST"
"""

import serial
import sys

def serial_write(cmd, port='/dev/ttyUSB0'):
    ser = serial.Serial(port=port, baudrate=9600, parity=serial.PARITY_NONE, stopbits=serial.STOPBITS_TWO, bytesize=serial.EIGHTBITS)
    if not ser.isOpen():
        ser.open()
    ser.write(cmd + '\r\n')
    ser.close()

if __name__ == '__main__':
    serial_write(sys.argv[1])
{% endhighlight %}

If the device is still not writeable (error message like "no permission to write"), add the user to the group where "/dev/ttyUSB*" is in.

## Reference:
* http://ubuntuforums.org/showthread.php?t=1409064
* [Examples of Python serial](http://stackoverflow.com/questions/676172/full-examples-of-using-pyserial-package)