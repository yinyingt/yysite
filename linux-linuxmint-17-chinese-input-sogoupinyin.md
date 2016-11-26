---
layout: post
title: Install Sogou Pinyin in Linux Mint 17
mathjax: false
related: false
comments: true
---

_Last modified: Jan, 2015_

Installing Chinese input method is one of the three biannual headaches when I upgrade my Linux, with the other two being wireless driver and graphic driver. While driver incompatibility can be mitigated by using older or well-supported hardware, Chinese input method is of no luck. So I guess installing a fledged Sogou Pinyin, a very popular input software in China, on my new Linux Mint 17 (based on Ubuntu LTS 14.04), deserves a separate post. Finally I bid an overdue fareware to the bitter-sweet companion of iBus over half a decade. 

This tutorial works in my Linux Mint 17 (64-bit), with the Sogou Pinyin Linux package of version 1.1.0.0037. 

* Install the language packages. Go to "Start" --> "Languages" --> "Install/Remove Languages", choose "Chinese". If some language packages are missing, install the missing packages. After that make sure the following components are installed:

{% highlight bash %}
sudo apt-get install language-pack-gnome-zh-hans* language-pack-zh-hans*
{% endhighlight %}

for simplified Chinese, and

{% highlight bash %}
sudo apt-get install language-pack-gnome-zh-hant* language-pack-zh-hant*
{% endhighlight %}

for traditional Chinese.

* Install Fcitx, which Sogou Pinyin depends on.

{% highlight bash %}
sudo add-apt-repository ppa:fcitx-team/nightly
sudo apt-get update
sudo apt-get install fcitx fcitx-bin fcitx-config-common fcitx-config-gtk fcitx-data fcitx-frontend-all fcitx-module-cloudpinyin fcitx-module-dbus fcitx-module-kimpanel fcitx-module-x11 fcitx-modules fcitx-qimpanel-configtool
{% endhighlight %}

* Download Sogou Pinyin Linux package from [here](http://pinyin.sogou.com/linux/?r=pinyin), and install it.

* "Start" --> "Input Method", select "fcitx" as the input method in the startup.

* "Start" --> "Fcitx Configuration", you may see the English keyboard is already there. Add input method (the little "+" button in the lower left corner), check off "only show current language" (because the current language is English and we want to show Chinese). There are lots of input methods to choose from. All you need to do is scrolling down to the very bottom, select "Sogou Pinyin", and click "OK".

* Restart X. And you should be able to see the pretty icon of Sogou Pinyin! 
