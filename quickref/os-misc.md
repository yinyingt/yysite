---
layout: post
title: Miscellaneous Problems about OS
mathjax: false
related: false
comments: true
published: true
---


__Last modified: Mar, 2016__


## How to disable touch screen of ThinkPad X220 Tablet in Windows 10

The option to disable touch screen in "Pen and Touch" (Windows 7) is gone in Windows 10. One needs to disable the drivers in Device Manager, as shown below: 

![device manager screenshot](http://i.imgur.com/ci61vXf.png)

Reference: [here](http://sachachua.com/blog/2013/02/disabling-touch-on-windows-8-on-a-lenovo-x220-tablet/).


## MS Windows Cannot Boot on A Dual-boot Computer

__Problem: Grub fails to boot Windows 7 in a Windows-Linux dual boot computer__

Solution: This may be due to a damaged MBR. 

Step 1) Boot from an installation Windows 7 DVD --> "Repair your computer" --> "Command prompt", and type

```
bootrec /fixmbr
bootrec /fixboot
```

More details can be found [here](http://www.tomshardware.com/news/win7-windows-7-mbr,10036.html)

Step 2) Reboot and check if the computer can boot Windows 7

Step 3) Follow instructions in [this link](http://people.virginia.edu/~ll2bf/docs/nix/grub_rescue.html) and install Grub. 


## MS Windows 7 Backup and Restore 

__Method 1: Using the built-in backup program__

Windows 7 has built-in program for backing up. To create a system image and save to another hard drive (such as an external hard drive), go to "Control Plane" --> "Create a system image", and follow the wizard. After that, it may prompt and ask if you want to create a recovery disc. Say "yes" and follow the wizard to create a system repair disc. Some graphical guide of this process can be found online, such as [here](http://www.howtogeek.com/howto/1838/using-backup-and-restore-in-windows-7/). 

To restore from a system image on another hard drive, boot into the system repair CD and select "select a system image backup". Attach to external hard drive having the system image, refresh the list, select the image and start restoring. If everything works, you just need to wait. Another graphical guide is [here](http://www.bleepingcomputer.com/tutorials/system-image-recovery-in-windows-7-8/). 

However, it doesn't always work (just check how people complain about the irregularities of this program). The key is once you create such a system image on another hard drive, just don't touch it. It turns out if you want to use the image, there are several requirements on this image folder (this [post](http://benz145.tumblr.com/post/69304546990/solution-windows-backup-restore-windows-cannot-find-a) also finds something similar): 

* 1. The image folder has to be named as "__WindowsImageBackup__" (which is the default name) 
* 2. The image folder has to be placed in the root of the hard drive (which is the default path) 
* 3. The image folder has to be read-only
* 4. There may be other pecularities (since people were complaining online that it still couldn't detect the external hard drive having the system image somehow)

Otherwise, things just don't work. This is annoying obviously.

__Method 2: using Clonezilla__

[Clonezilla](http://clonezilla.org) is a free and open-source Linux Live CD for disk imaging and cloning. Its ncurse-based interface makes it a little nostalgic, but this doesn't mean it is not powerful. 

I do a full hard drive raw cloning with Clonezilla, meaning the resulted image has the same size as my source hard drive. So do check if the destination has enough capacity before getting started! Although the Method 1 above may yields smaller size, the full hard drive imaging makes it somewhat platform independent (don't have to rely on Windows software to do the recovery), which is nice. 

Actually once you boot into the Live CD, the task flow is quite intuitive. Some useful graphical tours can be found [here](http://www.techrepublic.com/blog/windows-and-office/how-do-i-clone-a-hard-drive-with-clonezilla/2254/) and [here](http://www.dedoimedo.com/computers/clonezilla.html). 

Conclusion: if the destination hard drive has capacity constraint, go with Method 1. Otherwise, go with Method 2.


## How to remove Linux and GRUB on a dual-boot computer

[This post](https://www.techmesto.com/uninstall-linux-grub-dual-boot-windows8/) illustrates it well. In a nutshell, remove Linux partitions, boot with a Windows rescue disk/USB, prompt a command line and type

```
bootrec /fixmbr
```

## How to change Windows 7 display language

[This link](http://chiehwenynag.blogspot.com/2011/01/windows-7.html) says it all. 

## OSX related

__How to boot Mac from USB?__
 
Answer: Hold "Option" key while booting the computer. There will be an option for Windows. 


## Grub

__How to install multiboot system?__

There are scenarios where you need both Microsoft Windows and Linux(es) in one computer. There are some installation guidelines: 

* Always install Microsoft Windows first. If not, it will rudely erase the boot information of other operating systems previously installed. 

* Install a "clever" system that can handle multi-boot grub well after MS Windows. Ubuntu can be a good option. Install it disk-widely. For example, if you only have one disk '/dev/sda', then install the grub in '/dev/sda'.

* If you need to install more Linux(es), install each grub in its own partition. For example, if ArchLinux's '/boot' is in '/dev/sda7', then install the grub in '/dev/sda7'. 

* When the grub information changes in any system, use Ubuntu (the second system here) to update the grub disk-widely ('/dev/sda'). The commands are simple:

{% highlight bash %}
sudo update-grub   # probing OSes in this computer
sudo grub-install /dev/sda
{% endhighlight %}

__How to rescue a broken grub (grub2)?__

One can install/update grub from a LiveCD with Grub2

1. Boot with a LiveCD

2. Assume the root is in /dev/sda7 partition

{% highlight bash %}
sudo mkdir /media/root
sudo mount /dev/sda7 /media/root
sudo mount -t proc none /media/root/proc
sudo mount -t sysfs none /media/root/sysfs
sudo mount -o bind /dev /media/root/dev
sudo chroot /media/root /bin/bash
{% endhighlight %}

After that

{% highlight bash %}
sudo update-grub
{% endhighlight %}

That will interpret grub2 configuration file and write to '/boot/grub/grub.cfg'. And then,

{% highlight bash %}
sudo grub-install /dev/sda
{% endhighlight %}

to finish grub installation.

__System time is not corrected after daylight time savings in Linux__

This happened when I have a Windows-Linux dual boot computer. Installing and updating NTP did not help. The solution was to sync the BIOS clock by booting to Windows, and the boot to Linux. The system time in Linux was updated correctly after that.
