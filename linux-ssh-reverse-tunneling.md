---
layout: post
title: How to Access SSH Servers Behind Firewall with SSH Reverse Tunneling
mathjax: false
related: false
comments: true
published: true
---

_Created: Jul, 2015_
_Last modified: Jul, 2015_


Think of a case like this: Alice runs a server with public IP, and she wants to access Bob's server behind a firewall: 

Bob (bob@192.168.1.10, SSH port 22) <--> Firewall <--> Alice (alice@123.45.6.78, SSH port 2222). 

Although direct SSH won't work, she can still use SSH reverse tunneling. 

* Step 1: Bob initiates an SSH session to Alice 

```
ssh -p2222 -R 12345:localhost:22 alice@123.45.6.78
```

with the password to Alice's server. This will create an SSH tunnel between port 12345 (facing Alice) on Alice's server and port 22 on Bob's server. 

* Step 2: On Alice's server:

```
ssh -p12345 bob@localhost
```

with password to Bob's server. 

* Step 3: No further step. Done!


Here is another case: what if two sides (Bob and Alice) are both behind firewalls? Then a third server (Jim) with public IP should be involved. Bob initiates a SSH tunnel to Jim's server, and Alice can log in Jim's server to access Bob's server, which is very similar to the above.

One can also use [autossh](http://www.harding.motd.ca/autossh/) for this task like: 

```
autossh -M 0 -N -p2222 -R 12345:localhost:22 alice@123.45.6.78
```

Autossh will monitor the SSH link state, and take actions when exceptions happen (e.g., reconnect if the link is lost, etc.). 