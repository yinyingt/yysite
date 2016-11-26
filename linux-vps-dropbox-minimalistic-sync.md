---
layout: post
title: Enhance Linux VPS with Minimalistic Dropbox Sync
mathjax: false
related: false
comments: true
published: true
---



_Created: Apr, 2016_;

_Last modified: Apr, 2016_


Dropbox provides official proprietary daemon for Linux which works well in Desktop environment. However, for lean Linux VPS boxes with only command-line interface and limited RAM (and CPU), a better solution is needed. 

[Dropbox-Uploader](https://github.com/andreafabrizi/Dropbox-Uploader), a BASH based Dropbox API wrapper, comes to help. The sync is manual and half-duplex, but is a lean solution. 


## Installation

The only dependence is cURL

```
sudo apt-get curl
``` 

and BASH.

Download the tool:

```
git clone https://github.com/andreafabrizi/Dropbox-Uploader.git
```

and run the script "dropbox\_uploader.sh". If it is the first time to run this script, it will walk you through the process to setup a Dropbox app. For convenience, I grant access to the whole Dropbox folder. 

## Design and Action

My design is to setup a master VPS shared folder, within which there are different sub-folders for different VPS boxes. The master VPS shared folder will be further shared with my Desktop Dropbox client so that I can exchange files between my local Desktop and other VPS boxes. 

On my VPS, I create a file `~/.dropbox_addon_rc` with the content below

<script src="https://gist.github.com/lijunhw/eadbcffe746e758abea17eb026778aa0.js?file=DropboxUploader_bashrc_wrapper"></script>

and in `~/.bashrc` add

```
if [ -f ~/.dropbox_addon_rc ]; then
    . ~/.dropbox_addon_rc
fi
```

This will enable the half-duplex sync between local folder `$HOME/vps_shared_$HOSTNAME` and the remote Dropbox folder `/vps_shared/vps_shared_$HOSTNAME`. Note that, to make it more convenient, the names of local and remote folders are the same. Folder separation of different VPS instances is also implemented (but it is not the "isolation" in strict sense because each VPS box can still access the full Dropbox content). 

On the first time use, initiate the setup with

```
dropbox_uploader
```

and follow the instructions. This guide you to create an app key and secret, linking to your Dropbox account. I grant it the access to the whole Dropbox folder. 

To initialize the folder on Dropbox server side, do 

```
cd ~/vps_shared_$HOSTNAME
touch tmp.txt
dropbox_push
```

This will create a folder at `/vps_shared/vps_shared_$HOSTNAME` on the Dropbox server side. 

To pull contents from Dropbox server, type

```
dropbox_pull
```

and the content of the remote folder will **merge** with the local folder (meaning that the extra files will not be removed). So does `dropbox_push`. In that way, one can first pull and then push to sync the entire folder. 

To upload a single file or folder to Dropbox, do 

```
dropbox_push_single path_to_file_or_folder
```

Command `dropbox_clear_local` will remove all files in local folder (`~/vps_shared_$HOSTNAME`). Command `dropbox_clear_remote` will remove all files in the remote folder. These two are introduced when a fresh sync is needed. 

Command `dropbox_list` is essentially `dropbox_uploader list`, which will show a list of files on Dropbox remote folder.  

For more options, check out [README](https://github.com/andreafabrizi/Dropbox-Uploader/blob/master/README.md). For use cases (such as data backup), check out [its wiki](https://github.com/andreafabrizi/Dropbox-Uploader/wiki).
