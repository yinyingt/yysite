---
layout: post
title: Mutt Quick Ref
mathjax: false
related: false
comments: true
published: true
---

_Last modified: Apr, 2016_

*Reviving the good old ...*


## How to forward a message with attachments

By default, `f` will only forward the text-type message. To enable forwarding with attachments, do the following

* A) Add those lines to Mutt config file (maybe "`~/.muttrc`")

```
set mime_forward=yes
set mime_forward_rest=yes
```

* B) Open the message, press `v` to view the attachments, and press `t` to tag all attachments including (usually) the first text segment in the list (which is the text content of the message to be forwarded). 

* C) Press `;f` to invoke "tag-prefix" + "forward" which will prompt for a new message initialized with the header information of the message to be forwarded. 

* D) Edit (such as removing "End forwarded message") and save the message. In the main message composition window, there will be a list of content to be sent will be a) the content of header information of the message to be forwarded a) the text content of the message to be forwarded c) the attachments of the message to be forwarded. So there will be at least 3 items in the list. 

* E) Hit `y` to send. 

Reference: [Mutt Wiki: Attachment](https://dev.mutt.org/trac/wiki/MuttFaq/Attachment)