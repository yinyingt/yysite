---
layout: post
title: GPIB on Ubuntu 12.04
mathjax: false
related: false
comments: true
---

__Last modified: Jun, 2013__

I have been [using PyVISA to control GPIB lab instruments]({{ site.baseurl }}/blog/2013-06-01-pyvisa-v2.html). The only problem was I used NI-VISA as the VISA layer. Since I also used GPIB-USB-HS adapter from NI, [I have to use 32-bit Scientific Linux 5.x]({{ site.baseurl }}/posts/2013-06-01-pyvisa-v2.html).  While the whole package still looked much better than using the bloated LabVIEW, it was increasingly a pain because I couldn't have things like matplotlib working out of box in SL 5.x in the data processing stage. Yes, SL 5.x was a stable (legacy) system. But there were things (like Octave fonts, old Emacs, etc.) in SL 5.x making me feel annoyed from time to time. 

I was always thinking if I could have GPIB control from a much more modern system (sorry, SL 5.x grandpa) like Ubuntu 12.04, it would be great. Here is how. 

Download linux-gpib source from [here](http://linux-gpib.sourceforge.net/). Extract the tar ball, cd into the source directory, and do the routine compiling thing

{% highlight bash %}
./configure
make
sudo make install
{% endhighlight %}

After that, a group called "*gpib*" will be created. Add yourself to this group: 

{% highlight bash %}
sudo usermod -aG gpib username
{% endhighlight %}

Also link the GPIB library to library path: 

{% highlight bash %}
cd /usr/lib
sudo ln -s /usr/local/lib/libgpib.so.0 .
{% endhighlight %}

If you use GPIB-USB-HS adapters like me, go and edit */etc/gpip.conf* and change the *board_type* in *interface* section to 

{% highlight bash %}
board_type = 'ni_usb_b'
{% endhighlight %}

Now, if you plug in a GPIB-USB-HS cable to the laptop, you will see */dev/gpib[0~15]*. But they are owned root. To write to GPIB device in userspace, udev is the way to go. Create */etc/udev/rules.d/99-ni_gpib_usb.rules* with the content below: 

{% highlight bash %}
SUBSYSTEM=="usb", ACTION=="add", ENV{DEVTYPE}=="usb_device", 
ATTR{idVendor}=="3923", ATTR{idProduct}=="702[ab]", 
MODE="660", GROUP="gpib", SYMLINK+="usb_gpib"
SUBSYSTEM=="usb", ACTION=="add", ENV{DEVTYPE}=="usb_device", 
ATTR{idVendor}=="3923", ATTR{idProduct}=="702[ab]", 
RUN+="/lib/udev/ni_usb_gpib"
KERNEL=="gpib[0-9]*", ACTION=="add", MODE="660", GROUP="gpib"
{% endhighlight %}

I take this udev rule from [this page](http://www.cl.cam.ac.uk/~osc22/tutorials/gpib_usb_linux.html) and it just works like a charm after a reboot. What it does is udev recognizes the GPIB-USB-HS cable once it is plugged in, and make all GPIB devices writable to group *gpib*. 

At last, run

{% highlight bash %}
sudo gpib_config
{% endhighlight %}

Actually this command line should be run each time the linux-gpib drivers are loaded. If everything goes well, the GPIB communication should work by now. You can test it with an interactive GPIB tool (comes with linux-gpib) *ibtest*

{% highlight bash %}
ibtest
{% endhighlight %}

Note that you should be able to run it as a user in *gpib* group, otherwise udev is not configured right. Type 'd' --> type GPIB port --> type 'w' --> type '*IDN?' --> type 'r' --> enter, and you should read some equipment information back. 
The Python bindings of linux-gpib are enable by default during the compiling. To use it in Python, here is a quick start: 

{% highlight python %}
import gpib
h = gpib.dev(0, 20)  # Locate your GPIB device and get the device handle
gpib.write(h, '*IDN?')   
gpib.read(h, 1024)   # Read 1024 bytes max
{% endhighlight %}

You can also locate the device with *gpib.find()* with the device alias (a string) if you specify it in the *device section* of */etc/gpib.conf*. For more info, look at 

{% highlight python %}
help(gpib)
{% endhighlight %}
