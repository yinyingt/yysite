---
layout: post
title: ROS Notes
mathjax: false
related: false
comments: false
published: true
---






_Created: Dec, 2015_

_Last modified: Dec, 2015_


### Prepare a VirtualBox ROS image

I use Ubuntu-based [LXLE](http://lxle.net/) 14.04 (32-bit) which is made for resource constrained environment, and ROS [Indigo Igloo](http://wiki.ros.org/indigo) which is a LTS distribution supported till 2019.

First trim down the OS by removing unneeded application:

```
# Remove unneeded applications
sudo apt-get remove samba* fbreader* pithos* audacity* libreoffice* shotwell* gimp* gitso* transmission* homebank* anki* marble* keepassx* photoprint* parcellite* simple-scan* xfburn* arista* multibootusb* gparted* xpad

# Remove games
sudo apt-get remove --purge chromium-bsu* aisleriot* biniax2* criticalmass* dreamchess* flobopuyo* gweled* gnome-hearts* hex-a-hop* hexalate* lbreakout2* ltris* mahjongg* mastermind* gnome-mines* numptyphysics* pipewalker* primrose* solarwolf* swell-foop* teeworlds* zaz zaz-data

sudo apt-get autoremove

sudo apt-get update
sudo apt-get upgrade
```

And install useful applications

```
sudo apt-get install build-essential   # install C++ compiler and other build tools
sudo apt-get vim git
sudo apt-get install arduino arduino-core
```

Second, install ROS Indigo and configure the environment. See [ROS tutorials](http://wiki.ros.org/ROS/Tutorials) for more information.

```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
wget https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -O - | sudo apt-key add -
sudo apt-get update
sudo apt-get install ros-indigo-desktop-full
sudo rosdep init
rosdep update
echo "source /opt/ros/indigo/setup.bash" >> ~/.bashrc
sudo apt-get install python-rosinstall
```

Set the permission of "~/.ros" correctly: 

```
sudo chown -R username:groupname ~/.ros
```

### Use CMake with Arduino on ROS

This can be useful to managing the code for a larger project so that the firmware update can be committed without involving Arduino IDE. See [this tutorial](http://wiki.ros.org/rosserial_arduino/Tutorials/CMake). 

### Links

* [Limitations of rosserial](http://wiki.ros.org/rosserial/Overview/Limitations) on data types, publishers and subscribers.
