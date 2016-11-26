---
layout: post
title: Control Lab Instruments with PyVISA in Linux
mathjax: false
related: false
comments: true
---

_Last modified: May, 2013_

Update in Jun 2013: a newer version is available [here](./linux-pyvisa-v2.html) with a slightly different scenario and easier procedure. 

### Before installation

Required: 

* A computer with PCI-GPIB support
* Operating system: Scientific Linux 5
* [NI-VISA](http://www.ni.com/visa/), Linux version
* NI-488.2 GPIB driver, Linux version
* [Python ctypes](http://python.net/crew/theller/ctypes/)
* [PyVISA](http://pyvisa.sourceforge.net/)

The reason to use Scientific Linux (SL) is that it is recompiled from Red Hat Enterprises Linux (RHEL). If a third-party company releases products with Linux support, RHEL probably will be on the list. Being RHEL's derivative, SL has a great chance to enjoy the support. Actually NI-VISA officially supports both RHEL and SL 5 (they also support SUSE Linux but I don't use it). At the moment of writing, the latest SL is SL 6. But I install SL 5 in my lab computer. 

You don't need a license to access NI-VISA and NI-488.2 GPIB driver. All you need is registering an account in NI's website. Go to http://www.ni.com, find "'Download Software on the right side'" and click on "'Drivers and Updates'". At the moment of writing, I use **NI-VISA 5.0 Linux verion** and **NI-488.2 2.3 Linux version**.

NOTE: It seems NI-VISA only works with NI-488.2 GPIB driver. I tried http://linux-gpib.sourceforge.net/[Linux GPIB packages] but it didn't work. 

In SL 5.5, install the dependencies: 

{% highlight bash %}
yum install kernel-devel gcc make python-devel
{% endhighlight %}

### Installing NI components

PyVISA doesn't come with VISA libraries and drivers. That is why NI-VISA and NI-488.2 are needed. 

Installation is quite simple. Download the iso images (**NI-VISA** and **NI-488.2 GPIB driver**), mount and install like this:

{% highlight bash %}
su - 
mkdir /mnt/iso
mount -t iso9660 -o loop your-NI-image.iso /mnt/iso
cd /mnt/iso
# read README before proceeding
sh INSTALL
{% endhighlight %}

As said above, read README file before installtion. 

### Installing PyVISA

In SL 5.5 the default Python is 2.4 (old...), and the dependency of PyVISA, Ctypes is not included by default. So grab Ctypes source, extract, cd into the directory and 

{% highlight bash %}
sudo python setup.py install
{% endhighlight %}

to install Ctypes. After that, download PyVISA tar ball, extract, cd into the directory, and do

{% highlight bash %}
sudo python setup.py install
{% endhighlight %}

to install PyVISA. Pretty standard procedure for Python packages. 


### Testing

To see whether it is working, connect the computer with an instrument with GPIB cable. I have a Keithley source meter 2400, so I connect it with my computer, fire up a Python interactive shell, and type

{% highlight bash %}
import visa
keithley = visa.instrument("GPIB::25")
print keithley.ask("*IDN?")
{% endhighlight %}

NOTE: The communication port of my Keithley is set to '25'. You need to change it accordingly. 

and it says: 

{% highlight bash %}
KEITHLEY INSTRUMENTS INC.,MODEL 2400,1248860,C30   Mar 17 2006 09:29:29/A02  /K/J
{% endhighlight %}

It works!

### Appendix
Is there any alternative solution without having NI firmware involved? 

I quote from someone's email: 

"You could probably use Comedi drivers to run the GPIB board (google comedi), but perhaps the simplest option is "don't use VISA". For GPIB instruments, it really doesn't add much - just send/receive ASCII text to/from the GPIB instrument (full command list should be in the instruments user manual)." 
