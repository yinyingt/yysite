---
layout: post
title: Control Lab Instruments with PyVISA in Scientific Linux 5 (v2.0)
mathjax: false
related: false
comments: true
---

_Last modified: June, 2013_

This is an improved tutorial based on the [previous post](./linux-pyvisa-v1.html). Now the scenario is that I need to use a NI GPIB-US-HS adapter to control some lab equipments with GPIB.

### Before installation
Required: 

* A computer with USB support
* GPIB-USB-HS adapter from National Instrument
* Operating system: Scientific Linux 5.x, 32-bit
* [NI-VISA](http://www.ni.com/visa/), Linux version
* NI-488.2 GPIB driver, Linux version
* [PyVISA](http://pyvisa.sourceforge.net/)

Here are some words about OS choice: There is already Scientific Linux 6.x at the time of writing. So why still use SL 5? The reason is the official NI driver of GPIB-USB-HS adapter is only supported by Linux kernel up to 2.6.24 (see the README of NI-488.2 package). According to [this post](https://decibel.ni.com/content/thread/5214), Linux developers changed the license of USB driver APIs since kernel 2.6.25 which prevented proprietary drivers from using the kernel. If you want to go for NI drivers like I do, there is no choice but going for old kernels (<=2.6.24). Since SL 6.x comes with kernel > = 2.6.32, SL 5.x is the only choice left. Also remember to select 32-bit SL 5.x since 64-bit is not supported yet. 

Surely there are ways around. One example is to use the open-source [Linux-GPIB](http://linux-gpib.sourceforge.net)driver. I heard it worked pretty well for some people. But in this post I will focus on the NI driver sets. 

If you are using a PCI-GPIB card instead, you can install SL 6.x (easier to work with than SL 5.x). The rest of the tutorial stay the same. 


### Install NI drivers

First install dependencies

{% highlight bash %}
sudo yum install kernel-devel gcc glibc-devel make
{% endhighlight %}

Now install NI-488.2. Download the package from NI's website, unpack, and run 

{% highlight bash %}
sudo ./INSTALL
{% endhighlight %}

For NI-VISA, download the iso image from NI's website,

{% highlight bash %}
su 
mkdir /mnt/iso
mount -t iso9660 -o loop your-NI-image.iso /mnt/iso
cd /mnt/iso
# read README before proceeding
./INSTALL
{% endhighlight %}

Very standard procedures. 


### Install PyVISA

SL 5.x is old. The default Python version is 2.4. Instead of messing with ctypes in Python 2.4 as I did in the [previous post]({{site.baseurl}}/wiki/linux-pyvisa-v1.html), I find [pythonbrew](https://github.com/utahta/pythonbrew) is a nice tool to have multiple Pythons with different versions in the same box. 

First install the dependency:

{% highlight bash %}
sudo yum install readline-devel
{% endhighlight %}

Now follow the instructions on pythonbrew Github page and install it: 

{% highlight bash %}
curl -kL http://xrl.us/pythonbrewinstall | bash
{% endhighlight %}

and append 

{% highlight bash %}
[[ -s $HOME/.pythonbrew/etc/bashrc ]] && source $HOME/.pythonbrew/etc/bashrc
{% endhighlight %}

to file '$HOME/.bashrc', and re-source the bashrc file

{% highlight bash %}
source ~/.bashrc
{% endhighlight %}


Now I can easily install the newest Python in SL 5.x like

{% highlight bash %}
pythonbrew install 2.7.3
{% endhighlight %}

This will install Python 2.7.3 in pythonbrew's separated environment (located in '~/.pythonbrew'). Now use Python 2.7.3 as the default Python version:

{% highlight bash %}
pythonbrew switch 2.7.3
{% endhighlight %}

Even better, PyVISA is in PyPI (Python Package Index) repository now. So install PyVISA can be as easy as

{% highlight bash %}
pip install pyvisa
{% endhighlight %}

That's it. You can hook the computer up with an equipment and test as I did in the  [previous post]({{site.baseurl}}/wiki/linux-pyvisa-v1.html). 

NOTE: If you have RAM larger than 3GB, and have errors related memory mapping, you can append string like *mem=4096M* as the boot option in */boot/grub/grub.conf*. This solves the problem sometimes. This is because we are using 32-bit system here, and it cannot address RAM above 3GB. 
