---
layout: post
title: Use Bluetooth on Linux
mathjax: false
related: false
comments: true
published: true
---



_Created: Mar, 2015_;

_Last modified: Mar, 2016_


[Bluez](http://www.bluez.org/) is a popular Bluetooth toolbox for Linux. At the time of writing, I am using Linux Mint 17 and Raspberry Pi.


## Installation 

Not surprisingly, the BlueZ source in Ubuntu and Debian is old. To get the latest features (e.g., those in Bluetooth V4.2), one can compile the whole thing from the latest source code. 

First install the dependencies

{% highlight bash %}
sudo apt-get install libdbus-1-dev libdbus-glib-1-dev libglib2.0-dev libical-dev libreadline-dev libudev-dev libusb-dev make
{% endhighlight %}

Note that Bluez uses systemd which is not the default service manager in Ubuntu (it's upstart). So I just skip installing systemd packages on Linux Mint to avoid potential headaches. Raspberry Pi Debian Linux uses systemd, so it should be okay. Download the latest Bluez source from [Linux kernel's website](http://www.kernel.org/pub/linux/bluetooth/) or [Bluez download page](http://www.bluez.org/download/). At the time of writing, the latest version is 5.29. 

Next, install dependencies. For Ubuntu/Linux Mint

{% highlight bash %}
./configure --disable-systemd --enable-library
{% endhighlight %}

The first flag "--disable-systemd" disable compiling with systemd libraries since we will not use systemd anyway, otherwise you can get error like "configure: error: systemd system unit directory is required". The second flag "--enable-library" installs Bluetooth library so that it is possible to develop your own BT applications in the future. 

Finally, compile and install:

{% highlight bash %}
make
sudo make install 
sudo cp attrib/gatttool /usr/local/bin/
{% endhighlight %}

The last command installs _gatttool_ system-wide since it is not automatically done in "make install". 


## Play with Bluetooth Low Energy 

I am mostly interested in Bluetooth Low Energy (BLE) applications which becomes part of the Bluetooth stack since V4.0. Since my computer does not have BLE capability, I get a CSR4.0 BLE USB adapter from Amazon ([here](http://www.amazon.com/BATTOP%C2%AE-Bluetooth-Dongle-Chipset-Adapter/dp/B00IMALOZ0)) which works pretty well. Note that this device only implements BT V4.0 stack, thus not having new features in BT V4.1 and V4.2. 

Insert the CSR4.0 BLE USB dongle, and make sure you see something similar to the following after "lsusb":

{% highlight bash %}
Bus 002 Device 005: ID 0a12:0001 Cambridge Silicon Radio, Ltd Bluetooth Dongle (HCI mode)
{% endhighlight %}

View all BT devices on the computer by 

{% highlight bash %}
hciconfig
{% endhighlight %}

I see the following: 

{% highlight bash %}
hci1:	Type: BR/EDR  Bus: USB
	BD Address: 00:1A:7D:DA:71:13  ACL MTU: 310:10  SCO MTU: 64:8
	DOWN 
	RX bytes:3447 acl:0 sco:0 events:153 errors:0
	TX bytes:730 acl:0 sco:0 commands:59 errors:0

hci0:	Type: BR/EDR  Bus: USB
	BD Address: CC:52:AF:E1:F6:06  ACL MTU: 1021:8  SCO MTU: 64:1
	UP RUNNING PSCAN 
	RX bytes:1056 acl:0 sco:0 events:53 errors:0
	TX bytes:1433 acl:0 sco:0 commands:53 errors:0
{% endhighlight %}

It turns out hci0 is the native built-in BT module on my computer, and hci1 is the CSR4.0 BLE USB dongle. Right now hci1 is still in "down" mode while hci0 is running. Now bring hci1 up by 

{% highlight bash %}
sudo hciconfig hci1 up
{% endhighlight %}

After another "hciconfig hci1", you will see hci1 becomes: 

{% highlight bash %}
hci1:	Type: BR/EDR  Bus: USB
	BD Address: 00:1A:7D:DA:71:13  ACL MTU: 310:10  SCO MTU: 64:8
	UP RUNNING 
	RX bytes:4005 acl:0 sco:0 events:181 errors:0
	TX bytes:1085 acl:0 sco:0 commands:87 errors:0
{% endhighlight %}

You can also list the active BT devices by 

{% highlight bash %}
hcitool dev
{% endhighlight %}

which will return the MAC addresses of active BT devices attached to the computer, like 

{% highlight bash %}
Devices:
	hci1	00:1A:7D:DA:71:13
	hci0	CC:52:AF:E1:F6:06
{% endhighlight %}

Now we can do various things with hci1. For example, I have a [SensorTag](http://www.ti.com/tool/cc2650stk). I can scan the BLE devices by 

{% highlight bash %}
sudo hcitool lescan
{% endhighlight %}

It will continuously output information about BLE peripheral devices to the console like: 

{% highlight bash %}
BC:6A:29:AB:7B:1A SensorTag
BC:6A:29:AB:7B:1A SensorTag
BC:6A:29:AB:7B:1A SensorTag
...
{% endhighlight %}

Now I can test a BLE connection to my SensorTag by starting and dropping a connection like 

{% highlight bash %}
sudo hcitool lecc BC:6A:29:AB:7B:1A
{% endhighlight %}

and it will return something like 

{% highlight bash %}
Connection handle 72
{% endhighlight %}

meaning a connection was successful. Now close the connection: 

{% highlight bash %}
sudo hcitool ledc 72
{% endhighlight %}


## Configure Udev

By default the BLE dongle is inactive after being connected to the computer. Udev can help enable the device upon connection by adding this udev rule: 

{% highlight bash %}
ACTION=="add", KERNEL=="hci1", RUN+="/usr/local/bin/hciconfig hci1 up"
{% endhighlight %}

Note that the path for "hciconfig" may be different on different systems. Be sure to check the path with "which". 


## More 

The real fun part is to connect to a BLE peripheral like SensorTag and interact with "_gatttool_". [Jared Wolff's post](http://www.jaredwolff.com/blog/get-started-with-bluetooth-low-energy/) covers some basic usage. 

The CSR4.0 BLE USB dongle can also be used to capture the advertizing packet data from other BLE devices. See discussion in this [thread](http://stackoverflow.com/questions/21733228/can-raspberrypi-with-ble-dongle-detect-ibeacons/21790504#21790504). 

There is also a Python package for interfacing BLE on the shoulder of Bluez [here](https://github.com/IanHarvey/bluepy). The package seems still a little preliminary, but does provide the most needed basic features. 

One can make an iBeacon device with this little BLE USB dongle. For example, [piBeacon](https://learn.adafruit.com/pibeacon-ibeacon-with-a-raspberry-pi/overview) does exactly such thing.

__[Update: Mar 2016]__

I recommend a NodeJS package called [Noble](https://github.com/sandeepmistry/noble) for BLE scripting. I find it much easier to use than existing alternatives. 


## References

* [RPi Bluetooth LE](http://www.elinux.org/RPi_Bluetooth_LE)
* [Get started with Bluetooth Low Energy (from jaredwolff.com)](http://www.jaredwolff.com/blog/get-started-with-bluetooth-low-energy/)
* [How to resolve bluez configure:error:systemd system unit directory is required](http://askubuntu.com/questions/343663/ubuntu-13-04-and-bluez-5-8-configure-error-systemd-system-unit-directory-is-re)
