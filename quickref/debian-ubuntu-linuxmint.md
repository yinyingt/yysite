---
layout: post
title: "Quick reference of Debian, Ubuntu and Linux Mint"
mathjax: false
related: false
comments: true
published: true
---


_Last modified: Mar, 2016_


## Repositories

__Use Mirrors__

Sometimes use repository mirrors other than the official repository can be much faster. 

For Ubuntu repository, the mirror list can be found in this [link](https://launchpad.net/ubuntu/+archivemirrors). 

For Linux Mint repositories (which uses some Ubuntu repositories as well), one can get the fastest one via "Software Source" --> "LinuxMint Software" --> "Download from" --> "Other" --> "Select best server". 

The __/etc/apt/source.list__ for my Linux Mint 13 is 

{% highlight bash %}
deb http://mirror.umd.edu/linuxmint/packages/ maya main upstream import
deb http://mirror.anl.gov/pub/ubuntu/ precise main restricted universe multiverse
deb http://mirror.anl.gov/pub/ubuntu/ precise-updates main restricted universe multiverse
deb http://mirror.anl.gov/pub/ubuntu/ precise-security main restricted universe multiverse
deb http://archive.canonical.com/ubuntu/ precise partner
{% endhighlight %}

__Google Chrome Repository Error (updated 2016)__

Error message after "sudo apt-get update"

```
“Failed to fetch http://dl.google.com/linux/chrome/deb/dists/stable/Release
Unable to find expected entry ‘main/binary-i386/Packages’ in Release file (Wrong sources.list entry or malformed file)”
```

It may be because Google dropped the support for 32-bit version of Chrome browsers. The fix is to change the file at "/etc/apt/sources.list.d/google-chrome.list" as follows: 

```
deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main
```

It should work again. 

Reference: [How To Fix The (Annoying) ‘Failed to Fetch’ Chrome apt Error](http://www.omgubuntu.co.uk/2016/03/fix-failed-to-fetch-google-chrome-apt-error-ubuntu)


## 中文輸入法

__Linux Mint 13 Chinese Input__

對於本人而言， 用Linux的三大頭疼問題是無線網卡驅動、顯卡驅動和中文輸入法。之前試過ibus+pinyin, fcitx+pinyin, fcitx+sougou(見貼[1](http://the-1.info/install-fcitx-input-method-in-mint-16/),[2](http://blog.segmentfault.com/weakish/1190000000474999))。最後發現fcitx-rime乃是一個不頭疼的解決方案。可惜Linux Mint 13 (基於Ubuntu 12.04 LTS) 上沒有打包好的package, 不過一個PPA就可解決問題。

{% highlight bash %}
sudo add-apt-repository ppa:fcitx-team/nightly
sudo apt-get update
sudo apt-get install fcitx-rime
{% endhighlight %}

然後重啓X, 就能在Linux Mint 13裏正常用了。 


__Install Sogou Pinyin in Linux Mint 17__

This tutorial has been made a separate post [here](../linux-linuxmint-17-chinese-input-sogoupinyin.html)


## Hardware Problems 

__(LM13 Cinnamon) Problem: The touchpad of ThinkPad W510 doesn't work after waking up from a suspend__

Solution: reload the mouse kernel module as follows and it will work: 

```
sudo modprobe -r psmouse
sudo modprobe psmouse
```

__(LM13 Cinnamon) Problem: The brightness of ThinkPad W510 is not adjustable with 'Fn' keybindings__ 

The NVidia proprietary binary driver is installed. Similar problem also described [here](http://forums.linuxmint.com/viewtopic.php?f=208&t=106557&p=600748&hilit=thinkpad+laptop+brightness)

Solution: See the [answer in the some thread](http://forums.linuxmint.com/viewtopic.php?f=208&t=106557&p=600748&hilit=thinkpad+laptop+brightness#p602113) above. But the simplest way to fix this is to add an entry like ```Option "RegistryDwords" "EnableBrightnessControl=1"``` to __/etc/X11/xorg.conf__, making a section something like: 

```
Section "Device"
  Identifier "Default Device"
  Option "NoLogo" "True"
  Option "RegistryDwords" "EnableBrightnessControl=1"
EndSection
```
After a reboot, brightness adjustment with "Fn" key works.

__(LM13 XFCE) Problem: The system sound (except the login sound) doesn't work__ 

Solution: The problem may be because Linux Mint loads wrong sound driver. The following command can fix the problem for the current session: 

{% highlight bash %}
rm ~/.pulse && killall pulseaudio
{% endhighlight %}

To make this fix effective permanently, [this post](http://www.linuxandlife.com/2012/05/no-sound-in-linux-mint-13-maya.html) gives a fine solution: "Menu" --> "Settings" --> "Session and Startup" --> "Application Autostart" --> "Add", and type in above command to create a startup entry. So this command will be executed everytime the session starts.
