---
layout: post
title: Disk Encryption in Linux
mathjax: false
related: false
comments: true
published: true
---

_Created: Nov, 2015_;
_Last modified: Nov, 2015_

This post shows how to do full disk encryption in Linux, and how to decrypt on boot. It works on Ubuntu 14.04 (Linux Mint 13), but should be applicable to other Linux flavors with no or minor modifications.

The "dm_crypt" tool is used here by default. The basic idea is "dm_crypt" decrypts disk partitions before mounting them. Therefore the configuration file "/etc/crypttab" is read before "/etc/fstab". In this process, the encrypted partition will first be decrypted to file "/dev/mapper/XXXX", which will be subsequently mounted.

Before getting started, first check if "dm_crypt" tools are installed and the kernel module is loaded:

```
sudo apt-get install cryptsetup
sudo modprobe dm_crypt
```

## Root and home partition

I usually make root and home in a single partition during installation. Therefore this part of encryption has already been taken care of by checking encrypting home partition option during the installation of Ubuntu. This partition will be automatically decrypted on boot.


## Swap partition

Swap partition will also be automatically encrypted if home partition encryption option is checked during installation. It should work out of box except that it does not in my cases most of the time. The error is related to the non-existing UUID of the swap partition for some reason. The issue can be circumvented by replacing the UUID with disk file in "/etc/crypttab" like:

```
cryptswap1 /dev/sda1  /dev/urandom swap,cipher=aes-cbc-essiv:sha256
```

where "/dev/sda1" used to be a UUID. This will decrypt the swap partition at /dev/sda1 to /dev/mapper/cryptswap1. In "/etc/fstab" the related (default) entry is:

```
/dev/mapper/cryptswap1  none  swap sw 0 0
```

Finally, enable swap if it is not active yet:

```
sudo swapon -a
```

## None-root partition initialization

For the first-time set-up, the following procedure will erase the existing data on the partition. Assume /dev/sda3 is the non-root partition to be encrypted.

First [wipe the partition securely](https://wiki.archlinux.org/index.php/Dm-crypt/Drive_preparation#Secure_erasure_of_the_hard_disk_drive).

After that, set up the LUKS header on the partition with default options

```
sudo cryptsetup -v luksFormat /dev/sda3
```

See [this link](https://wiki.archlinux.org/index.php/Dm-crypt/Device_encryption#Encryption_options_for_LUKS_mode) for a full list of options. During this operation, a passphrase is created to encrypt and decrypt the partition.

Now open the partition by

```
sudo cryptsetup open device nonroot
```

This will decrypt the partition to /dev/mapper/nonroot. There is still no file system on this partition it yet. Create it by

```
sudo mkfs.ext4 /dev/mapper/nonroot
```

and then mount it

```
sudo mount -t ext4 /dev/mapper/nonroot /mnt/mypart
```

Optionally, to close and un-mount this partition, do

```
sudo umount /mnt/mypart
sudo cryptsetup close nonroot
```

## None-root partition automatic decryption and mounting on boot

The idea is very similar to swap partition. First add a record to "/etc/crypttab":

```
nonroot  /dev/sda3   none  luks,timeout=60
```

This means to prompt for passphrase ("none" option) and decrypt /dev/sda3 to /dev/mapper/nonroot with the timeout of 60 seconds. Correspondingly, in "/etc/fstab", add an entry like

```
/dev/mapper/nonroot  /mnt/mypart  ext4  defaults,errors=remount-ro  0  2
```


## SSD consideration

TRIM is not enabled by default here. One way to enable TRIM is to [add "discard" option to each abstraction layer of file system](http://blog.neutrino.es/2013/howto-properly-activate-trim-for-your-ssd-on-linux-fstrim-lvm-and-dmcrypt/) (meaning need to apply it to both "/etc/crypttab" and "/etc/fstab"). However, that may bring non-negligible file system overhead since the defrag is carried out right after each flash erase. Probably not the best option.

A more recent tweak introduced in Ubuntu 14.04 by default is that the system runs a cron job doing "fstrim" weekly (weekly seems to be a reasonable interval after [some discussion](http://wiki.ubuntuusers.de/SSD/TRIM)). This approach can make the file system configuration fields "discard"-free since the job is already being done by "fstrim" on the background.

Another caveat is "fstrim" cron job is configured for Samsung and Intel SSD by default. To enable for SSDs from other vendors, add "--no-model-check" flag to fstrim in "/etc/cron.weekly/fstrim"

```
exec fstrim-all --no-model-check
```


## Links

* [Arch wiki: dm-crypt/Encrypting a non-root file system](https://wiki.archlinux.org/index.php/Dm-crypt/Encrypting_a_non-root_file_system)

* [Arch wiki: dm-crypt/System configuration](https://wiki.archlinux.org/index.php/Dm-crypt/System_configuration)

* [How to properly activate TRIM for your SSD on Linux: fstrim, lvm and dm-crypt](http://blog.neutrino.es/2013/howto-properly-activate-trim-for-your-ssd-on-linux-fstrim-lvm-and-dmcrypt/)

* [AskUbuntu: How is Trim enabled?](http://askubuntu.com/questions/443761/how-is-trim-enabled)

* [SSD: how to optimize your Solid State Drive for Linux Mint 17.2, Ubuntu 14.04 and Debian](https://sites.google.com/site/easylinuxtipsproject/ssd)
