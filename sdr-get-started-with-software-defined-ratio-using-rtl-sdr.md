---
layout: post
published: true
title: Get Started with Software Defined Radio using RTL-SDR
mathjax: false
related: true
comments: true
---

_Last updated: Feb, 2015_

[Software defined radio (SDR)](http://en.wikipedia.org/wiki/Software-defined_radio) is really something fun play with. The SDR equipment used to be expensive, but the hardware got cheaper as time went by. SDR became even more accessible when someone discovered [a ~$20 TV tuner using Realtek chip has a very wide tuning range](http://superkuh.com/rtlsdr.html). After that, enthusiastic people quickly flow in and developed software (e.g., [rtl-sdr](http://sdr.osmocom.org/trac/wiki/rtl-sdr)), which opens the door for various applications and fun, which worth way more than $20. The ["About RTL-SDR" page on rtl-sdr.com](http://www.rtl-sdr.com/about-rtl-sdr/) make a pretty clear explanation.

This tutorial documents how I set up my Rafael Micro R820T USB dongle with [RTL2832U](http://www.realtek.com.tw/products/productsView.aspx?Langid=1&PNid=22&PFid=35&Level=4&Conn=3&ProdID=257) chip inside, which works between 24~1766 MHz. In the end, it should be able to pick up radio signals in your local area. 

Hardware needed: 

* A $20 Rafael Micro R820T tuner bought from [NooElec](http://www.nooelec.com/store/). You can also get it from elsewhere (e.g., eBay, AliExpress, etc.), maybe with a lower price. 
* A Linux computer. I use LinuxMint 17. 


## Build & Install RTL-SDR Tools

The Osmocom people have a very nice page [here](http://sdr.osmocom.org/trac/wiki/rtl-sdr) on using RTL-SDR software. The information below works for me. 

First install dependencies:

{% highlight bash %}
sudo apt-get install libusb-1.0-0-dev git cmake
{% endhighlight %}

Here I use cmake to compile binaries. After that, grab rtl-sdr source:

{% highlight bash %}
git clone git://git.osmocom.org/rtl-sdr.git
{% endhighlight %}

and compile

{% highlight bash %}
cd rtl-sdr/
mkdir build
cd build
cmake ../ -DINSTALL_UDEV_RULES=ON -DDETACH_KERNEL_DRIVER=ON   # this enables non-root use
make
sudo make install
sudo ldconfig
{% endhighlight %}

Note that I have cmake two flags to enable the non-root use (by adding udev rules) and [resolve kernel conflicts](http://www.raspberrypi.org/forums/viewtopic.php?f=41&t=81731).

It is time to test whether this R820T USB dongle works. Run: 

{% highlight bash %}
rtl_test -t   # Note that you don't need to be root to run this command
{% endhighlight %}

and I get the output

{% highlight bash %}
Found 1 device(s):
  0:  Realtek, RTL2838UHIDIR, SN: 00000001

Using device 0: Generic RTL2832U OEM
Detached kernel driver
Found Rafael Micro R820T tuner
Supported gain values (29): 0.0 0.9 1.4 2.7 3.7 7.7 8.7 12.5 14.4 15.7 16.6 19.7 20.7 22.9 25.4 28.0 29.7 32.8 33.8 36.4 37.2 38.6 40.2 42.1 43.4 43.9 44.5 48.0 49.6 
Sampling at 2048000 S/s.
No E4000 tuner found, aborting.
Reattached kernel driver
{% endhighlight %}

E4000 is another RTL2823U dongle (Elonics E4000) which has wider band, but is not what we have. The output indicates the software recognizes our device. 

A really quick demo can be done with "rtl_fm" command line, which can be used to listen to a local FM radio station: 

{% highlight bash %}
rtl_fm -f 89.3e6 -M wbfm -s 200000 -r 48000 - | aplay -r 48k -f S16_LE
{% endhighlight %}

It listens to a FM radio at 89.3MHz. You can search for your local FM radio channel list [here](http://radio-locator.com/)


## Use GNU Radio

The good thing is GNU Radio packages are already in Ubuntu 14.04's repositories. 

{% highlight bash %}
sudo apt-get install gnuradio gr-osmosdr
{% endhighlight %}

Here _gr-osmosdr_ is an Osmocom plugin for GNU Radio. At the time of writing, the version of the stable GNU Radio binary in Ubuntu 14.04 is 3.7.2, while the latest version is 3.7.6. 

Open GNU Radio Companion (named "GRC" in the start menu), and construct a signal flow chart by adding (dragging) a few blocks from the component panel on the right to the work space:

* "Sources" --> "osmocom Source"
* "Instrumentation" --> "WX" --> "WX GUI FFT Sink"

This signal flow chart will do a FFT analysis on the receiving channel. Note that GNU Radio is a fast-evolving software. The GUI interface may change in the future. But the general idea of signal flow chart remains the same. 

Double click on the source and sink to configure their parameters. In the end, click "Build" --> "Generate" (or simply press "F5"), and then click "Build" --> "Execute" (or press "F6"). A real-time FFT plot will come up. 

There are lots other (meaningful) things that can done with GNU Radio. Check out the GNU Radio tutorial page [here](https://gnuradio.org/redmine/projects/gnuradio/wiki/Tutorials). 


## Links

* [rtl-sdr.com](http://www.rtl-sdr.com): blog and lots of information about RTL-SDR
* [RTL-SDR page on OsmoSDR](http://sdr.osmocom.org/trac/wiki/rtl-sdr). By the way, OsmoSDR is a free software based small form-factor inexpensive SDR project. 
* [RTL-SDR and GNU Radio with Realtek RTL2832U (Elonics E4000/Raphael Micro R820T) software defined radio receivers](http://superkuh.com/rtlsdr.html): lots of useful information on what you can do with RTL-SDR; still regularly updated. 
* [Getting Started with GNU Radio and RTL-SDR (on Backtrack)](http://blog.opensecurityresearch.com/2012/06/getting-started-with-gnu-radio-and-rtl.html)
* [GNU Radio](http://gnuradio.org)
