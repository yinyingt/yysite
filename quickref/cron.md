---
layout: post
title: Quick Reference of Cron
mathjax: false
related: false
comments: true
published: true
---


_Last modified: Aug, 2011_


## View cron job entries 

To list cron job entries of root

{% highlight bash %}
sudo crontab -l
{% endhighlight %}

To view current user's cron job entries 

{% highlight bash %}
crontab -l
{% endhighlight %}

To view other users':

{% highlight bash %}
sudo crontab -u other_username -l
{% endhighlight %}


## Edit cron job entries

To edit root's:

{% highlight bash %}
sudo crontab -e
{% endhighlight %}

__Entry syntax__

The basic template is as follows

```
*     *     *   *    *        command_to_be_executed
-     -     -   -    -
|     |     |   |    |
|     |     |   |    +----- day of week (0 - 6) (Sunday=0)
|     |     |   +------- month (1 - 12)
|     |     +--------- day of        month (1 - 31)
|     +----------- hour (0 - 23)
+------------- min (0 - 59)
```

Besides, there are some special usages. For example, 


```
*/10 * * * * command_to_be_executed
```
means executing this command every 10 minutes; and 

```
@hourly command_to_be_executed
```
means to execute command every hour, which is equivalent to 

```
0 * * * * command_to_be_executed
```

Similarly, @daily = 0 0 * * *, @monthly = 0 0 1 * *, @yearly = 0 0 1 1 *, and @reboot means executing at boot every time self-explanatorily.   


## Log Cron Jobs

Cron job output may already be logged in "/var/log/syslog". To log cron job output to a separate file such as "/var/log/cron.log", change the rsyslog settings as decribed in [this post](http://askubuntu.com/a/121560).  


## Reference

There are tons of cron tutorials online. Here are just a sip.

* [Linux Cron job 15 examples](http://www.thegeekstuff.com/2009/06/15-practical-crontab-examples)
* [Crontab quick reference](http://adminschoice.com/crontab-quick-reference)
* [Intro to cron](http://unixgeeks.org/security/newbie/unix/cron-1.html)
