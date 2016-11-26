---
layout: post
title: Vagrant Notes
mathjax: false
related: false
comments: true
published: true
---

_Created: Mar, 2016_

_Last modified: Apr, 2016_


Vagrant is a tool piggy-backing on top of a VM provider such as VirtualBox or VMWare. The good thing  of using Vagrant is to separate the development environment from the source files. The development environment can be configured with Vagrantfile, and can be __provisioned__ with an automated job (e.g., a BASH script) to enforce environment consistency from a vanilla box. Therefore, a developer can have development environment consistency and flexibility at the same time. 

What makes it more powerful is that a developer can combine Vagrant and Docker together so that he can serve multiple Docker containers inside one Vagrant box. 


## Installation and Configuration

Install VirtualBox first. And then go to [Vagrant download page](https://www.vagrantup.com/downloads.html) and install the latest Vagrant tool. 

Before proceeding, have a look at some [Vagrant environmental variables](https://www.vagrantup.com/docs/other/environmental-variables.html). Specifically, "VAGRANT_HOME" points to the directory holding Vagrant boxes, which can be very large on disk. The default is "~/.vagrant.d". Point this path to a partition with abundant disk space. 

The Vagrant provisioning scripts can be stored in individual project space. 

Vagrant boxes can be downloaded through [VagrantCloud](http://vagrantcloud.com) (supported by HashiCorp), or via some 3rd-party sites like [vagrantbox.es](http://vagrantbox.es). 


## Bring-up

First download a Vagrant box by 

```
vagrant box add ubuntu/trusty64
```

which will download the latest 64-bit Ubuntu 14.04 box from the VagrantCloud to folder "$VAGRANT_HOME".  

Now to the project folder, and create a Vagrant box profile

```
cd ~/vagrant_boxes/test/
vagrant init ubuntu/trusty64
```

which will create a Vagrantfile with default configuration options. Finally, bring up the Vagrant box in this folder with 

```
vagrant up --provider virtualbox
```

Since I only install VirtualBox on my computer, the "--provider virtualbox" part can actually be omitted. This command will boot the virtual Linux box and bring up a ssh daemon, and doing some boiler-plate task such as creating a vagrant user upon the first-time boot. To use this box, do 

```
vagrant ssh
```

which will ssh into this box with vagrant user (who has sudo privilege). 

By default, the project folder will be mapped to "/vagrant" inside the Vagrant box, so that one can edit those files inside the Vagrant box but all the changes will be saved without changing the Vagrant box environment itself. After booting to a vanilla Vagrant box, if one wants to [provision](https://www.vagrantup.com/docs/getting-started/provisioning.html) this box (configure the environment) with a BASH script at some point, do 

```
vagrant provision
```

The path of provisioning job can be configured in Vagrantfile, such as

```
config.vm.provision "shell", path: "vm_provision/boot.sh"
```

Note that the changes induced by provisioning will persist in the next boot. 

By default `vagrant up` will not run the provisioning job unless it is its first boot. One can enforce to provision (or re-provision) on boot by

```
vagrant up --provision
```

To shutdown the current running Vagrant box, do 

```
vagrant halt
```

To restart a Vagrant box to reload Vagrantfile, do 

```
vagrant reload
```

which is essentially the same to `vagrant halt` followed by `vagrant up`. 

Finally, one can remove this Vagrant VM instance with 

```
vagrant destroy
```

This will remove the instance together with all provisioned changes. But the files (including Vagrantfile) in the project folder still remain. 


## Other commands

Below are some commonly used Vagrant commands: 

* To list all Vagrant boxes, do 

```
vagrant box list
```

* To check the status of Vagrant boxes

```
vagrant status
```

* Suspend this Vagrant VM:

```
vagrant suspend
```

* Resume the previously suspended Vagrant VM

```
vagrant resume
```

For more command line options, view the Vagrant documentation [here](https://www.vagrantup.com/docs/cli/). 


* To make a Vagrant access USB device, configure according to [this post](http://code-chronicle.blogspot.com/2014/08/connect-usb-device-through-vagrant.html). 
