---
layout: post
title: Raspberry Pi Quick Ref
mathjax: false
related: false
comments: true
published: true
---


_Last updated: Oct, 2015_

## How to burn OS image to SD card for Raspberry 

OS images are available in its [official website](http://www.raspberrypi.org/downloads/). In Linux, the most convenient way is to use command "dd": 

```
sudo dd if=sd.img of=/dev/mmcblkX bs=4M
```

Note that "/dev/mmcblkX" is the device file for the corresponding SD card. 

One interesting thing I found was when using dd command above, setting "bs=1M" will not result in a working OS (e.g., Raspbmc, OpenElec). Instead, "bs=4M" works. 

## How to shrink Raspberry Pi image

If one needs to transfer a Raspberry Pi image from a large SD card to a smaller SD card, downsizing as follows may help:

1. Clone the SD card content to an image with Win32DiskImager (in Windows) or dd (in Linux). Say it is named as rpi.img.

2. If in Linux, download [this script](https://github.com/lijunhw/llpi/blob/master/scripts/rpi_img_downsizer.sh) and run

```
sudo bash rpi_img_downsizer.sh rpi.img
```

This will decrease the image to the 110% of its minimum size. 

If in Windows, one use install Cygwin and follow the [instructions here](http://smartretro.co.uk/forums/viewtopic.php?t=58).


## Change system fonts to Chinese in Raspbmc

Check out [this video](https://www.youtube.com/watch?v=DZH72uIefQE).

## Survive wifi disconnection

In the default Raspbian OS, Pi won't reconnect to wifi once it is disconnected for some reason (e.g., router restart), assuming the set up information is in _/etc/network/interfaces_. This is how to do it in a dirty way. 

Create a script with the following content

```bash
#!/bin/bash
if ! [ "$(ping -c 1 www.google.com)" ]; then
    ifdown wlan0
    ifup wlan0
fi 
```

and save it to a place like "/home/pi/restart_wlan0.sh".

Open crontab with 

```bash
crontab -e
```

And add a cron job like

```
*/3 * * * * sudo bash /home/pi/restart_wlan0.sh
```

so that the cron job will run once every 3 minutes, assuming the current user can run sudo commands without password (if not, use cron with root).

Finally, restart cron

```
sudo /etc/init.d/cron restart
```

## Make Raspberry Pi push updates to Twitter

This can be a neat feature for Raspberry Pi automation. 

* The first step is to set up the Twitter authorization so that it can accept the push from Raspberry Pi. Go to [apps.twitter.com](https://apps.twitter.com/) to register an app. The API key and API secret are already generated. And one also needs to generate access token and access secret. In "keys and access tokens" tab, generate access token and access secret. 

* Install "twython" using pip. 

* Install security packages to avoid "insecure platform" warning: 

```
sudo apt-get install libffi-dev libssl-dev
[sudo] pip install requests[security]
```

as discussed in this [post](http://stackoverflow.com/questions/29134512/insecureplatformwarning-a-true-sslcontext-object-is-not-available-this-prevent). 

* Check out [my RPi libraries here](https://github.com/lijunhw/llpi/tree/master/pylib) on how to make use of it.
