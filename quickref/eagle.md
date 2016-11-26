---
layout: post
title: Quick Reference of Eagle
mathjax: false
related: false
comments: false
---

_Last modified: Dec, 2010_


[Eagle 5 Manual](../../assets/files/wiki/eagle_5_manual_en.pdf) and [Eagle 5 Tutorial](../../assets/files/wiki/eagle_5_tutorial_en.pdf) are two useful documents from CadSoft. 


## Library Glitches (Fedora 13)

When installing EAGLE in Fedora 13 (and also in Fedora 14), you may encounter error like

```
/tmp/eagle-setup.21333/eagle-5.10.0/bin/eagle: error while loading shared libraries: 
libssl.so.0.9.8: cannot open shared object file: No such file or directory
```

However, you may find 'libssl.so' is right inside '/usr/lib', which is linked to another libssl.so with a different version (e.g., 'libssl.so.1.0.0a'). At least this simple cheat helps in my Fedora 13: 

```
cd /usr/lib
sudo ln -s libssl.so libssl.so.0.9.8
```

Eagle also needs 'libcryto.so.0.9.8'. It is available in package 'openssl-devel', which is not installed by default: 

```
sudo yum install openssl-devel
sudo ln -s libcrypto.so libcrypto.so.0.9.8
```

## Installation on 64-bit Ubuntu 12.04

One can get the latest Eagle software from [CadSoft's website](http://www.cadsoftusa.com/). The installation needs 32-bit libraries as follows: 

```
sudo apt-get install libssl1.0.0:i386 libexpat1:i386 libfontconfig1:i386 libfreetype6:i386 libxrender1:i386
sudo apt-get install libjpeg62:i386 libstdc++6:i386 libx11-6:i386 libxau6:i386 libxcb1:i386 
sudo apt-get install libxcursor1:i386 libxdmcp6:i386 libxext6:i386 libxfixes3:i386 libxi6:i386 libxrandr2:i386 
```

This works for Eagle 6.0.5 on 64-bit Ubuntu 12.04. 


## Some Eagle Commands

TIP:
 
* A right-click on an object will open up the options available.  
* Command is usually followed by a left-click to select the object which the command is going to operate on. For example, command 'group' should be followed by a mouse selections of group elements. 

Always consult 

```
help [command name]
```

for details. 

* __display__ (or short for 'dis'): check layer notation
* __dis all__/__dis none__: show all layers / show none layer
* __dis 23__: display Layer 23
* __dis -23__: toggle off Layer 23
* __dis last__: restore the last layer selection
* __group__ (or short for 'gro'): make changes to a group of elements
* __add__: add schematic parts
* __use *__: enable all libraries that can be found in the path
* __grid 0.25__: switch to 0.25inch grid
* __name__: name an object (such as buses, pins, nets, etc.)
NOTE: By default, Eagle names buses with 'B$...', pins with 'P$...', and nets with 'N$...'.

* __delete__ (or 'del' for short): delete object
* __show__: show object (nets, components, etc.) details in the status bar
NOTE: Issuing command 'show' followed by selecting a net component you may see some other parts in the schematic that are in the same net hightlighted.
* __smash__: break apart a component (to change the text of a component, the size of a component, etc.)
* __value__: set the value of resistor, capacitor, inductor, etc.
* __erc__: invoke electrical rule check (ERC). 
* __route__ (or __rou__ for short): manual route in the layout.
* __ratnest__ (or __rat__ for short): calculate the shortest airwires and polygons. 


## Update Library Components On The Fly

This happens to me a lot when I use customized EAGLE libraries of my own: the physical layout of the customized libraries evolves as the project goes on, although it may look exactly the same in schematic view. I used to delete the old components in the schematic and add the updated components. However, it messes up the PCB layout a little bit.  

The better way is to directly add the updated component without deleting the old ones at first. EAGLE then asks you whether you need to update the component library. You say yes. It then can automatically update without extra deleting-adding operation, and the components in PCB layout are updated as well. 


## Modify Desgin Rules (DRC)

Typing "drc" in EAGLE's CLI should bring up the Design Rules Check(DRC) window. However, sometimes I can not change the default values for some unknown reasons (at least it is the case for my EAGLE in Linux...). 

There is a way around it: copy the default dru file (it should be in "_$HOME/.eagle/dru/default.dru_") to another place such as "_$HOME/local/eagle/mydru_", edit the file and change the related parameters. For my DIY projects in which I usually fabricate PCBs on my own, probably the parameters "_mdWireWire_", "_mdWirePad_" and "_mdPadPad_" are what I am mostly interested in. Save the file. Type "_drc_" in EAGLE's CLI, click "File" --> "Load" to load the customized dru file, and click "Select". Type command "rat" in board view, and you will see the difference. 

