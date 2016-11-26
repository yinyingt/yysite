---
layout: post
title: Quick Reference of GnuPG
mathjax: false
related: false
comments: false
published: true
---

_Last modified: Apr, 2010_


## Generating keys

To generate a GPG private-public key pair, run

```
gpg --gen-key
```

It is an interactive and self-explanatory process. 


## Listing keys

To list public keys:

```
gpg --list-keys
```

and too list private keys: 

```
gpg --list-secret-keys
```

## Encryption
 
To encrypt a file without a public key, run

```
gpg -c foo.txt
```

and enter a passphrase. 

To encrypt a file with a public key, run

```
gpg --encrypt --recipient <email-address> foo.txt
```

The _<email-address>_ should be replaced by the corresponding email address registered with the desired public key. 


## Decryption

To decrypt a file which was encrypted without a public key, run

```
gpg foo.txt.gpg -o <file-decrypted>
```
and enter the passphrase. 

To decrypt a file which was encrypted with a public key, run

```
gpg --output foo.txt --decrypt foo.txt.gpg
```

or 

```
gpg --decrypt foo.txt.gpg > foo.txt
```


## Key import and export

To import a public key, run

```
gpg --import public.key 
```

To import a private key, run

```
gpg --allow-secret-key-import --import private.key 
```

To export a public key, run

```
gpg --export -a user@example.com > public.key 
```

To export a private key, run

```
gpg --export-secret-keys -a user@example.com > private.key 
```


## Signing and verifying signature

To sign a file with Person A's own public key, A should do

```
gpg --clearsign msg.txt
```

That will gives a new signed file called msg.txt.asc which contains both the content and signature. "--clearsign" means the signature is in clear text. 

For Person B, to verify A's signature on a file, he should do

```
gpg --verify msg.txt.asc
```

given that B has the public key of A in his keyring. Here B can verify if msg.txt.asc is really from A using A's public key. 



## Getting fingerprint of a private key

```
gpg --fingerprint user@example.com 
```


## Deleting keys

Delete a public key, run

```
gpg --delete-keys user@example.com 
```

Delete a private key, run

```
gpg --delete-secret-keys user@example.com 
```

## Change the passphrase of a private key: 

```
$ gpg --edit-key [keyID]
gpg> passwd
Enter passphrase: 
Enter the new passphrase for this secret key.
Enter passphrase:
Repeat passphrase:
gpg> save
```

NOTE: if you keep a copy of private key with original passphrase somewhere else, the encrypted file with the updated privated key can still be decrypted by the old private key. Only private key matters, and passphrase is irrelevant. So it is extremely important to keep the private key secure in your hands. 


## Extending expired keys

http://tech.michaelaltfield.net/wp/?p=414


## Run gpg-agent

.In GNOME

At least the following works for Ubuntu (GNOME). 

According to _"man gpg-agent"_ (also [here](http://www.gnupg.org/documentation/manuals/gnupg/Invoking-GPG_002dAGENT.html)), GPG agent can be started together with X by adding this line into _$HOME/.xsession_

```
eval $(gpg-agent --daemon)
```

And don't forget to add 

```
use-agent
```

to "'$HOME/.gnupg/gpg.conf'". 

.In LXDE

However, in my Feodora 14 LXDE (as well as Lubuntu 10.10), it seems that file "'$HOME/.xsession'" is not read when X is started. So I just add the following into "'$HOME/.bashrc'": 

```
################
# Start the GnuPG agent and enable OpenSSH agent emulation
# https://wiki.archlinux.org/index.php/Using_SSH_Keys#Using_GnuPG_Agent

if pgrep -u "${USER}" gpg-agent >/dev/null 2>&1; # see if gpg-agent daemon is running
then
echo > /dev/null # do nothing 
else
    eval `gpg-agent --enable-ssh-support --daemon &> /dev/null` # start gpg-agent daemon with ssh support
fi
```

What it does is to start 'gpg-agent' when it is not there, or do nothing if it has been started. Note that 'ssh-agent' daemon is also started along with 'gpg-agent'. If you have a ssh key, you need to add the key to the keyring by

```
ssh-add
```

It will guide you through the key adding process. For more ssh key manipulation, consult "_ssh-add(1)_". 

NOTE: Again, don't forget to add "**use-agent**" to file "'$HOME/.gnupg/gpg.conf'", or 'gpg-agent' just won't work. 


__Reference__

* 'gpg-agent(1)' or [http://www.gnupg.org/documentation/manuals/gnupg/Invoking-GPG_002dAGENT.html](official manual)
* [https://wiki.archlinux.org/index.php/Using_SSH_Keys#Using_GnuPG_Agent](Using GnuPG Agent)
* __ssh-agent(1)__
* __ssh-add(1)__

## "pub" and "sub" (or "sec" and "ssb")

The difference has been discussed in the post [here](http://ubuntuforums.org/showthread.php?t=680292&page=2). 

And I quote: 

>Every key on a keyring there is at least one signing and one encryption key. The signing key is the primary key, the encryption key is the subkey."

To see the difference, 

>```
>Public KeyRing
>pub 1024D/069C39A4 2008-01-28
>uid John Smith <johnsmith@hismail.com>
>sub 2048g/045D39H5 2008-01-28
>
>Private Keyring
>sec 1024D/069C39A4 2008-01-28
>uid John Smith <johnsmith@hismail.com>
>ssb 2048g/045D39H5 2008-01-28
>```
>
>The public signing key is designated by pub 1024D/069C39A4. The corresponding private signing key is sec 1024D/06C39A4. 
>
>The public encryption key is designated by sub 2048g/045D39H5. The corresponding private encryption key is ssb 2048g/045D39H5.
"


## Run GPG from a different directory

By default, GPG reads keys from "$HOME/.gnupg". But if one use an external drive to use GPG, this default directory can be modified by set 

```
export GNUPGHOME=/path/to/keys
```

in "$HOME/.bashrc".


## Links

* http://aperiodic.net/phil/pgp/tutorial.html
