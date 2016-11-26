---
layout: post
title: Use Linux on Chromebook
mathjax: false
related: false
comments: true
published: true
---


_Created: Apr, 2015_

_Last Updated: Apr, 2016_


Benefits of Chromebooks: 

* Lightweight, compact and low-cost 
* Long battery life

Make it run on Linux will be perfect. I used to use [Crouton](https://github.com/dnschneid/crouton). But as of 2016, [GalliumOS](https://galliumos.org/) turns out to be a better option for me because it is a full-blown Linux on Chromebook hardware. 


## Gallium OS

Go to [GalliumOS Wiki](https://wiki.galliumos.org/Welcome_to_the_GalliumOS_Wiki) for instructions. It is likely you need to open the Chromebook case to unblock the 3rd-party firmware flashing. 


## Install Crouton in Chromebook

_[Update 2016] I no longer use Crouton because I prefer a full-fledged Linux. But it doesn't mean Crouton is not a good option for you._

[Crouton](https://github.com/dnschneid/crouton) is a chroot environment piggy-backing on the ChromeOS (aka, it is not a full Linux). Because of the limited memory space on Chromebook, I usually install Crouton in an external USB drive as follows. 

* First enable developer's mode. This will result in a complete reset of the current ChromeOS including all of the data. 

* Insert USB drive, create a "chroot" directory and symbolically link to "/usr/local"

```
cd /usr/local
sudo mkdir /media/removable/USB_DRIVE_NAME/chroots
sudo ln -s /media/removable/USB_DRIVE_NAME/chroots chroots
```

This is because Crouton installer will look for and install binaries to "/usr/local/chroots" by default. 

* Download Crouton via [https://goo.gl/fd3zc](https://goo.gl/fd3zc) as said in [Crouton's Github](https://github.com/dnschneid/crouton). 

* Install a distribution with selected desktop environment: 

```
cd /home/user/XXXX/Downloads
sudo sh crouton -r trusty -t cli-extra  # this ditches X environment
```

To have a list of supported distributions, run: 

```
sh crouton -r list
```

To have a list of available desktop environments, run: 

```
sh crouton -t help
```

For more command line options, refer to [Crouton command line cheatsheet @ Crouton Wiki](https://github.com/dnschneid/crouton/wiki/Crouton-Command-Cheat-Sheet). 

* To start Linux in chroot, there are two ways. To start Linux without GUI, do 

```
sudo enter-chroot
```

To start Linux with LXDE, do

```
sudo startlxde
```

The former is quicker than latter.
