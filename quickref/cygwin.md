---
layout: post
title: Quick Reference of Cygwin
mathjax: false
related: false
comments: false
---

_Last modified: Mar, 2012_

Cygwin is an excellent Unix emulating environment in Windows. 


## Run ssh-agent under Windows 

However, ssh-agent doesn't naturally run in Cygwin shell. Here are some treaks to get it work. 

* 1. Add these lines to file __$HOME/.bashrc__: 

{% highlight bash %}
export SSH_AUTH_SOCK=/tmp/.ssh-socket
ssh-add -l 2>&1 >/dev/null

if [ $? = 2 ]; then
   # Exit status 2 means couldn't connect to ssh-agent; start one now
   ssh-agent -a $SSH_AUTH_SOCK >/tmp/.ssh-script
   . /tmp/.ssh-script
   echo $SSH_AGENT_PID >/tmp/.ssh-agent-pid
fi

function kill-agent {
   pid=`cat /tmp/.ssh-agent-pid`
   kill $pid
}
{% endhighlight %}

* 2. Add an environmental variable "__SSH_AUTH_LOCK__", and set its value to "__/tmp/.ssh-socket__": Click 'Start' --> 'My Computer' --> Right click --> 'Properties' --> 'Advanced' --> 'Environmental Variables' --> 'New'. 

* 3. To use 'ssh-agent', open a Cygwin session, and run

```
ssh-add
```

* To kill this process, run "'kill-agent'" in Cygwin shell. 


## Open Cygwin terminal here with right click

It is quite handy if adding Cygwin terminal to the right click menu. Here is how. 

* 1.Install 'chere' package in Cygwin by running its "setup.exe".

* 2.Open a Cygwin terminal as the __Administrator__(right click, and "run as Administrator"), and type

```
chere -i
```

to install __chere__ to the right-click menu. 

Now right click on any folder and there will be "Bash prompt here". (__Note that right click on an empty space will not give "Bash prompt here"__.)

