---
layout: post
title: Use Bus Pirate on Linux
mathjax: false
related: false
comments: true
published: true
---

_Last modified: Mar, 2015_

This tutorial works for [Bus Pirate](http://dangerousprototypes.com/docs/Bus_Pirate) V3 on LinuxMint 13, but should also work for other Linux distributions with or without minor modifications. 

## Set UDev Rules

UDev can make Bus Pirate show up with a consistent device file under /dev. First find the device information associated with the Bus Pirate device (more details can be found in [udev quick reference](./quickref/udev.html)). Run

{% highlight bash %} 
lsusb
{% endhighlight %}

and find the related information such as 

{% highlight bash %} 
Bus 002 Device 009: ID 0403:6001 Future Technology Devices International, Ltd FT232 USB-Serial (UART) IC
{% endhighlight %}

It means the device is at "/dev/bus/usb/002/009". And run 

{% highlight bash %} 
udevadm info -a -p $(udevadm info -q path -n /dev/bus/usb/002/009)
{% endhighlight %}

Create a udev rule with some information in the output above, such as 

{% highlight bash %} 
SUBSYSTEMS=="usb", ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6001", \
    ATTRS{serial}=="A9015CAY", MODE:="0666", \
    SYMLINK+="BusPirate1"
{% endhighlight %}

Here the serial number attribute is added to the udev rule in case there are other FTDI chips interfacing with the host computer. Save this to "**/etc/udev/rules.d/61-buspirate1.rules**", and restart the udev daemon

{% highlight bash %} 
sudo restart udev
{% endhighlight %}

Reconnect the Bus Pirate to the computer, and it should be associated with "/dev/BusPirate1", and writable to all. 


## Interface with Bus Pirate with PySerial

_update, Mar 2015: I found miniterm may send extra characters to Bus Pirate. Using minicom or screen is recommended for now._

Bus Pirate interfaces with host computer with text-based commands via UART link. 

It is natural for a Python fan like myself to resort to Python-based tools for almost everything. Here a command-line based Python script called "miniterm.py", which is a part of [pySerial](http://pyserial.sourceforge.net/) package, is used to interface with Bus Pirate. First install the pySerial package as follows

{% highlight bash %} 
# updated in Mar, 2015, for LinuxMint 17
# used to be sudo apt-get install python-pyserial 
sudo apt-get install python-serial  
{% endhighlight %}

The usage of miniterm can be found [here](http://pyserial.sourceforge.net/examples.html#miniterm). According to [this post](http://dangerousprototypes.com/docs/Bus_Pirate_101_tutorial), the UART parameters for Bus Pirate are

* Baud rate: 115200
* Data bits: 8
* Parity bit: none
* Stop bit: 1
* Flow control: none

Now run

{% highlight bash %} 
miniterm.py -b 115200  /dev/BusPirate1
{% endhighlight %}

After an "Enter", the CLI should be very similar to that in [this tutorial](http://dangerousprototypes.com/docs/Bus_Pirate_101_tutorial#Get_to_know_the_terminal_interface). 

A self-test described in [this post](http://dangerousprototypes.com/docs/Self-test_guide) is recommended before the first use to see if the Bus Pirate board is in good condition. 

In the end, enter "Ctrl+]" to exit.

## Interface with Bus Pirate with Minicom

Another popular UART tool is minicom: 

{% highlight bash %} 
minicom -b 115200 -D /dev/BusPirate1
{% endhighlight %}

But this is not done yet. I find that one has to disable hardware and software flow control to avoid sending extra characters to Bus Pirate in the UART communication. In this case, the windows may just hang there after the above command. Now press "Ctrl+A" and then "Z" (for help), and then "O" (configure minicom), select "serial port setup", press "F" (disable hardware flow control), and exit. Or, you can just do

{% highlight bash %} 
minicom -s
{% endhighlight %}

to set up the serial port interactively. 

Now you should be able to talk to the Bus Pirate normally.

## Interface with Bus Pirate with Screen

{% highlight bash %} 
sudo screen /dev/BusPirate1 115200
{% endhighlight %}

and you are good to go. 


## Use Bus Pirate

Tutorials are readily available. Here are just a few: 
 
* General: [Bus Pirate 101](http://dangerousprototypes.com/docs/Bus_Pirate_101_tutorial), [Bus Pirate 102](http://dangerousprototypes.com/docs/Bus_Pirate_102_tutorial)

* UART: [Bus Pirate Wiki: UART](http://dangerousprototypes.com/docs/UART), [Bus Pirate UART guide](http://dangerousprototypes.com/bus-pirate-manual/bus-pirate-uart-guide/)