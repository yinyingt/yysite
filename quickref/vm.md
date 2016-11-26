---
layout: post
title: Virtual Machines FAQ
mathjax: false
related: false
comments: false
published: true
---

__Last modified: May, 2014__


## VMWare Player

* Installation: One common problem is that VMPlayer quit even after a successful installation on Linux Mint 13. The solution is install its dependency first, and then install the binary. To install the dependency: 

{% highlight bash %}
sudo apt-get install build-essential linux-headers-$(uname -r)
{% endhighlight %}

* Uninstallation: First get the current VMPlayer product name by 

{% highlight bash %}
vmware-install -l
{% endhighlight %}

and then uninstall the corresponding product, such as 

{% highlight bash %}
sudo vmware-install -u vmware-player
{% endhighlight %}

* Problem: Bridged networking doesn't work on a VMPlayer guest OS inside a Linux host with wifi connection

Solution: The problem is probably the default bridged interface in VMPlayer eth0 (or the first one it can find). The wifi interface is probably not eth0. To bridge network to a specific interface, follow this [link](http://joekuan.wordpress.com/2012/12/12/vmplayer-how-to-bridge-to-a-specific-network-port/). 

* Problem: When booting a VM, error "Binary translation is incompatible with long mode on this platform. Disabling long mode. Without long mode support, the virtual machine will not be able to run 64-bit code. "

Solution: This means the 64-bit support is not enabled on the host machine. It can be enabled in BIOS. Restart the computer, and boot into BIOS. Under "Security" --> "Intel (R) Virtualization Technology", make it "enabled". Reference: https://communities.vmware.com/message/2133628#2133628

* How to install VMware Tools to an Ubuntu guest. The following works on a Lubuntu 14.04 guest. First install the dependencies on the guest instance: 

{% highlight bash %}
sudo apt-get install linux-headers-generic build-essentials
{% endhighlight %}

Attach the VMware Tools (in VMware Player, "Virtual Machine" --> "install VMware Tools"), and then there will be an iso image mounted to the guest's CDrom. Extract the file named "VM*.tar.gz" to some place, and cd into "vmware-tools-distrib" directory in the extracted directory. The file "manifest.txt" includes all the instructions of installation. But the simpliest approach is to follow the default: 

{% highlight bash %}
sudo ./vmware-install.pl
{% endhighlight %}

Reboot the machine to finish the installation. 


## VirtualBox

* Problem: USB is not recognized by the VirtualBox guest. 

Solution: First make sure "**dkms**" is installed. And add the current user to "vboxusers" group: 

{% highlight bash %}
sudo usermod -aG vboxusers myusername
{% endhighlight %}

Don't forget to log out or reboot to make change take effect.

* Problem: Windows guest machine cannot get connected to the Internet

Solution: "Settings" --> "Network" --> "Enable Network Adapater" and select "NAT". Under the "Advanced", select the right "Adapter Type" (try another if one doesn't work) and select "Cable Connected". Start the virtual machine, and Internet should get connected. 

If it still cannot access the Internet (although local connection is established), try to ping a public IP such as 8.8.8.8 (Google's public DNS). If the ping works, then it is probably the DNS problem. Click the local connection icon on the task bar --> "Support" --> "Details" --> change DNS to 8.8.8.8 and 8.8.4.4. 