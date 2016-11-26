---
layout: post
title: Yet Another Mutt Setup
mathjax: false
related: false
comments: true
published: true
---




_Created: Mar, 2016_;

_Last modified: Apr, 2016_


Mutt is a good old lean mail client flying under the radar in this chaotically hyped digital era. I have an [old post]({{ site.baseurl }}/old/nix/mutt_setup.html) configuring Mutt with external IMAP and SMTP programs. Now I feel it should be "leaner". The goal of post is to set up Mutt with

* No dependency on external programs: aka, use the built-in IMAP and SMTP support of Mutt
* Easy handling of multiple accounts

My configuration can be found in [this repo](https://github.com/lijunhw/mutt_config). PGP configuration is on my TODO list. 



## Installation

In Debian or Ubuntu, don't install mutt from repo because it involves too many other libraries, and is also probably too old. Instead, compile it from source. 

First, install dependencies with this command: 

```
sudo apt-get install build-essential libncurses-dev libssl-dev libtokyocabinet-dev libsasl2-modules libsasl2-dev elinks 
```

which is itemized with explanation as follows: 

```
sudo apt-get install build-essential    # install make and gcc etc.
sudo apt-get install libncurses-dev 
sudo apt-get install libssl-dev   # openssl libraries, needed for IMAP TLS connection
sudo apt-get install libtokyocabinet-dev  # needed for header_cache option below
sudo apt-get install libsasl2-modules  libsasl2-dev  # needed to use built-in SMTP function, see http://j.mp/1RWKLAf
sudo apt-get install elinks  # for viewing HTML in Mutt; more later
```

And then download [Mutt](http://www.mutt.org/download.html), compile it and install

```
# those options are disabled by default, but important to make mutt receive and send emails correctly
./configure --enable-pop --enable-imap --enable-smtp --with-ssl --enable-hcache --with-sasl --with-gnutls --enable-pgp --enable-debug --enable-nls
make
sudo make install
```

This will install mutt to `/usr/local/bin/mutt` with the desired features (IMAP, SMTP, caching, etc.) enabled.



## Multiple Account Access


If the mail service supports using app-specific passwords, use it! For Gmail and Yandex Mail, you will be rejected when using master password (the one you use to log in the web mail) in a 3rd-party mail client such as Mutt for good security reasons. 

[My Mutt configuration template repo](https://github.com/lijunhw/mutt_config) makes setting up Mutt simple:


```
git clone https://github.com/lijunhw/mutt_config.git
mv mutt_config ~/.mutts
cd ~/.mutts
mkdir rc signatures
cp rc_templates/gmail rc/gmail_john  # make an instance of gmail config in rc_templates
vim rc/gmail_john  # do you editing here
# optional 
# vim signatures/default.sig
mutt -F ~/.mutts/rc/gmail_john
```

The idea is to create different "muttrc" files for different accounts, and access those mail accounts in different sessions with 

```
mutt -F ~/.mutts/rc/gmail_john
```

Many people also use the "folder hook" and "account hook" features of Mutt to bring multiple accounts in a single Mutt session, but I just find my way easier and less confusing. 

Notice that this configuration templates also contains a mailcap file which automatically renders HTML emails with elinks in Mutt. See [this section](https://wiki.archlinux.org/index.php/mutt#Viewing_HTML) for details. 


## Using Mutt

Mutt shows keyboard binding at the top of CLI view, with quite amount of VIM-like key bindings. 

Search message in Mutt: very similar to vim. For example,`/` for search, `n` for search next, etc.

Mutt will keep deleted IMAP messages in the view until the program exit. To "purge" them right away, press `$`. See [this post](http://unix.stackexchange.com/a/120544). 



## Further Reading

* [Mutt on ArchLinux wiki](https://wiki.archlinux.org/index.php/mutt): cover almost all aspects of using Mutt. 
* [Calmar on Mutt (for dummies or however)](http://www.calmar.ws/mutt/)
* [Mutt wiki: Use IMAP](https://dev.mutt.org/trac/wiki/MuttGuide/UseIMAP)
* [Mutt syntax](https://dev.mutt.org/trac/wiki/MuttGuide/Syntax)
* [Mutt: Multiple Gmail IMAP Setup](https://zuttobenkyou.wordpress.com/2010/11/05/mutt-multiple-gmail-imap-setup/)
