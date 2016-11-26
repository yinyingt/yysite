---
layout: post
title: Android Quick Ref
mathjax: false
related: false
comments: false
published: true
---



_Created: 2014_

_Last modified: Dec, 2015_

## How to install custom ROM in Sero 7 Pro

Several custom ROMs are available in xda-developers website. I use dopa2.0 which can be downloaded from [here](http://forum.xda-developers.com/showthread.php?t=2470998). Note that dopa2.0 is the old Android 4.2 without the bloatware, and every aspect about hardware (HDMI, NFC, etc.) is still working. 

* First root the device and install CWM by following [the instruction](http://forum.xda-developers.com/showpost.php?p=46196739&postcount=2). I use WinXP virtual machine. ADB usb driver needs to be installed via device manager, and ignore the MTP driver installation inquiry. Execute the "_step0-DoItAll_" file to finish the root. During the process, the tablet may take several reboot. After each reboot, reconnect the tablet to PC (or virtual machine) manually.

* In windows, copy the ROM file to Sero 7 Pro via adb

{% highlight bash %}
adb push \path\to\file\sero7pro-bld60220-dopa-v2.0.zip /sdcard2/
{% endhighlight %}

Note that the trailing slash after "/sdcard2" should not be omitted, declaring it is a directory (reference [here](http://www.londatiga.net/it/how-to-use-android-adb-command-line-tool/)).

* Power off the tablet. And then press the power button and volume down key at the same time for a few seconds until see string "Recovery" appear on the screen. Release the keys. Use volume up and down keys for navigation and power button for confirmation.

* Select install zip from external sdcard, and enable ROM integrity check by selecting "toggle signature verification". Install the ROM and reboot. Detals can be found [here](http://www.phonearena.com/news/How-to-boot-into-custom-recovery-like-CWM-or-TWRP-on-Android_id54490).


## How to install Cyanogenmod ROM to Amazon Fire 7 tablet

The unexpensive [Amazon Fire 7 tablet](http://www.amazon.com/Fire-Display-Wi-Fi-GB-Includes/dp/B00TSUGXKE) is a good testbed for Cyagnogenmod. The whole installation process is documented in [this video](https://www.youtube.com/watch?v=sVv1D_LNLTg), but here are quick steps: 

* Download this [supertools](http://forum.xda-developers.com/amazon-fire/development/amazon-fire-5th-gen-supertool-root-t3272695) for 5th-generation Amazon Fire 7, and unzip. This tool pack includes necessary Android utilities such as adb and fastboot, USB drivers and also custom made batch job scripts. 

* Visit [CM-12.1 page for Amazon Fire 7](http://forum.xda-developers.com/amazon-fire/orig-development/rom-cm-12-1-2015-11-15-t3249416), and download CM ROM, OpenGapps (I use the "pico" version) and SuperSU. Prepare an external memory (microSD card or OTG USB drive) and copy these zip files to the external memory. Mount the external memory onto tablet. 

* Enable developer mode, and enable USB debugging. Connect tablet to PC via a USB cable, and install USB drivers if necessary. 

* If a recovery utility has not been installed yet, run "1-Amazon-Fire-5th-gen.bat" file in the supertool package, and install the recovery utility. Or one can also follow the [instructions here](http://forum.xda-developers.com/amazon-fire/orig-development/twrp-recovery-t3242548) to install TWRP, or [instructions here](http://forum.xda-developers.com/amazon-fire/orig-development/recovery-cyanogen-recovery-2015-11-04-t3240726) to install the Cyanogen Recovery.  

* Boot tablet to bootloader mode via 

```
adb reboot bootloader
```

in command line. Or do it manually by turning off the tablet first, and turning it on with holding volume down button after that. In the end, run "2-Boot-TWRP-To-Restore-Device.bat" to boot tablet to recovery mode. Note that one has to use command line to boot this Amazon tablet to recovery mode, because at least at the time of the writing the recovery cannot be flashed to the Amazon tablet since the bootloader is locked. 

* In the recovery mode, first back up the existing file system. Wipe out the existing ROM, and then install the zip images with the recovery utilities.

* Reboot. Viola.

## ADB

* A "friendly" list of ADB shell command can be found in [ADBShell.com](http://adbshell.com/). They are very similar to Linux commands. 
