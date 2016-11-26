---
layout: post
title: Quick Reference of LVM
mathjax: false
related: false
comments: false
---

_Last modified: Jan, 2010_


## Grow logic volume

The basic steps are like this: check free space --> expand logic volume --> expand file system. It can be done even with file system mounted. 


* 1. Check the mounted file system with
{% highlight bash %}
df -h 
{% endhighlight %}

* 2. Check the available logic volumes

```
sudo lvdisplay 
```

* 3. Check how much free space available in this volume group for the growth

```
sudo vgdisplay 
```

* 4. Now resize the logic volume up to some point (of course within the free space limit)

```
sudo lvresize -L +1GB /dev/mapper/vg-xxx/yyy 
```

* 5. Check the new size of the logic volume

```
sudo lvdisplay 
```

* 6. Expand the existing file system to the size of logic volume

```
sudo resize2fs -p /dev/mapper/vg-xxx/yyy 
```

This command works for both ext2 and ext3 file systems.


## Shrink logic volume

The order of shrinking is just the reverse of expanding: umount the file system --> check file system integrity --> shrink file system --> shrink logic volume.

* 1. First unmount the file system

```
sudo umount /yyy 
```

and get rid of any “device is busy” stuff.

* 2. Check the file system integrity

```
sudo e2fsck -f /dev/mapper/vg-xxx/yyy 
```

* 3. Shrink the file system to some point

```
sudo resize2fs /dev/mapper/vg-xxx/yyy 2000M 
```

Here it goes down to 2G, but above the physical size of existing files. This command works for both ext2 and ext3.


* 4. Shrink the logic volume

```
sudo lvresize -L -1G /dev/mapper/vg-xxx/yyy 
```

Here the logic volume is shrunk by 1G. The left space should not be smaller than that of the file system. Do the math.

* 5. Expand the file system to fit the shrunk logic volume

```
sudo resize2fs -p /dev/mapper/vg-xxx/yyy 
```

* 6. Remount the file system.



## How to grow logic volume to the maximum available space

```
sudo lvresize -l +100%FREE 
```
