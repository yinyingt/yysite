---
layout: post
title: Quick Reference of UDev
mathjax: false
related: false
comments: true
published: true
---

_Last modified: May, 2014_


## Find Device Information

First, get device file name (e.g., can be from /var/log/messages, lsusb, etc.). Assume the name is /dev/bus/usb/001/002, we can do 

```
udevadm info -q path -n /dev/bus/usb/001/002
``` 

This will return the device file path, something like "/devices/pci0000:00/xxx/xxx" which is a little long sometimes. After that, run 

```
udevadm info -a -p /devices/pci0000:00/xxx/xxx
```

which will return all (very verbose) information regarding this device. 

The above two commands can be combined as follows

```
udevadm info -a -p $(udevadm info -q path -n /dev/bus/usb/001/002)
```

## Reference

* [How to write udev rules](http://hackaday.com/2009/09/18/how-to-write-udev-rules/)
