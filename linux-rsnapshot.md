---
layout: post
title: Data Backup with Rsnapshot
mathjax: false
related: false
comments: true
---

_Last modified: Jan, 2012_

Rsync is great. But sometimes I overwrite files by mistake with rsync, and there is just no way back. Rsnapshot is a great tool to overcome such a drawback by storing snapshots in the timeline. 

### 1. Installation

I just do 

{% highlight bash %}
sudo apt-get install rsnapshot
{% endhighlight %}

on my Ubuntu. You can also compile it from source by following instructions [here]({{site.baseurl}}/assets/files/wiki/rsnapshot-HOWTO.en.pdf). 


### 2. Configuration

In rsnapshot configuration file, remember that:

* "**TAB**" is used to separate configuration keys and values in one line, rather than "**SPACE**". It is because this program tries to be cross-platform and Windows file names often contain spaces. It just don*t want to mess with "spaces". 

* All directories are ended with slashes ("/"). Otherwise things will go wrong. 

Now The task is to backup a local directory "*/data/bigdump/*" periodically to another directory (in another drive) "*/backup/.rsnapshots/*". 

#### 2.1. Configure system programs

The rsnapshot package in Ubuntu comes with a configuration file */etc/rsnapshot.conf*. Change entries upon your needs. I remove comment mark "#" in front of those entries in my simple case. 

{% highlight bash %}
snapshot_root	/backup/.rsnapshots/
cmd_cp		/bin/cp
cmd_rm		/bin/rm
cmd_rsync	/usr/bin/rsync
cmd_logger	/usr/bin/logger
cmd_ssh		/usr/ssh
cmd_du		/usr/bin/du
link_dest	1
verbose		2
loglevel	3
logfile		/var/log/rsnapshot.log
lockfile	/var/run/rsnapshot.pid
{% endhighlight %}

This helps rsnapshot to find the necessary facilities to do the job. Enabling "*link_dest*" is recommended for rsync 2.5.7 and later. 


#### 2.2. Configure backup folders

It is as simple as adding this line to the configuration file

{% highlight bash %}
backup	/data/bigdump/	localhost/
{% endhighlight %}

where "*/data/bigdump/*" is the source, and "*localhost/*" is the destination directory relative to "*snapshot_root*" above. 

You can make multiple lines containing "backup" keywords to back up more than one folder. 

Using the name "*localhost/*" is a good idea because rsnapshot can also be used to back up remote directories to local hard drive. Each host should be a separate folder. 



#### 2.3. Configure backup intervals

This is an important step. I choose

{% highlight bash %}
retain	a	6
retain	b	7
retain	c	4
{% endhighlight %}

I use a general name "a", "b" and "c" here rather than "hourly", "daily" and "weekly" in the example configuration file is because it makes things less confusing at the first time. 

Here is the explanation. Each "*retain*" lines (or key word "*interval*" as an alias of "retain" but "interval" is going to be obsolete) specify a backup scheme, and the number following it is the number of snapshots the program keeps under this scheme. If I manually run (once)

{% highlight bash %}
rsnapshot a
{% endhighlight %}

It will push the current source directories to "*rsnapshot_root*" (specifically, "*/backup/.rsnapshots/a.0/localhost/data/bigdump/*"), and delete the oldest snapshot ("*/backup/.rsnapshots/a.5/localhost/data/bigdump/*"), and rotate everything with version "+1" in the between, meaning, the previous "*/backup/.rsnapshots/a.0/localhost/data/bigdump/*" becomes "*/backup/.rsnapshots/a.1/localhost/data/bigdump/*", and so on. 

The point is, rsnapshot doesn't care when you run, but only the number of snapshots to be rotated. Each time you run, it will make such a rotation. 

Similarly, 

{% highlight bash %}
rsnapshot b
{% endhighlight %}

will keep 7 snapshots, and rotate them each time the scheme "*b*" is evoked by the command. 

Another important thing to mention is that you must arrange the order of "*backup*" entries in such a way that the job with smaller interval goes before that with larger interval. The reason is that the "*backup*" entry "*b*" will not pull directly from the source directory, but from the oldest snapshot defined by entry "*a*" (which is above entry "*b*"). More specifically, each time "rsnapshot b" is run, it will pull "*/backup/.rsnapshots/a.5/localhost/data/bigdump/*" to "*/backup/.rsnapshots/b.0/localhost/data/bigdump/*", and rotate the rest snapshot of "*b*" scheme. 

It will make sense if rsnaptshot works as cron jobs, as shown in the next section.


#### 2.4. Testing

Do a syntax check of "*/etc/rsnapshot.conf*"

{% highlight bash %}
rsnapshot configtest
{% endhighlight %}

You pass if it says "Syntax OK"

### 3. Working as cron jobs

After done with the configuration above, you can use cron daemon to automate things. For example, open up crontab

{% highlight bash %}
crontab -e
{% endhighlight %}

and edit like

{% highlight bash %}
0 */4 * * *   /usr/bin/rsnapshot a
30 23 * * *   /usr/bin/rsnapshot b
{% endhighlight %}

Basically those two lines means run rsnapshot with scheme "*a*" every 4 hours, and scheme "*b*" at 23:30 everyday. Refer to [this page]({{site.baseurl}}/docs/quickref/cron.html) for a quick check of cron syntax.

And guideline of adding more than one rsnapshot cron jobs is making a job with larger interval run a little before that with smaller interval. In this case, the first job is run in the beginning of hour every four hours (making the full rotation period 24 hours). The second job is run 30 min before the end of the day so that it can catch "*/backup/.rsnapshots/a.5/localhost/data/bigdump/*" can push it to "*/backup/.rsnapshots/b.0/localhost/data/bigdump/*". 

### 4. Real example

These are what I use on my computer. I am not using root privilege to do the sync, thus the "*logfile*" and "*lockfile*" are set to the places where the user has right to write. 

"*/etc/rsnapshot.conf*": 

{% highlight bash %}
snapshot_root	/backup/.rsnapshots/
cmd_cp		/bin/cp
cmd_rm		/bin/rm
cmd_rsync	/usr/bin/rsync
cmd_logger	/usr/bin/logger
cmd_ssh		/usr/ssh
cmd_du		/usr/bin/du
link_dest	1

retain		rh_two	2
retain		rd_seven	7
retain		rw_four		4
retain		rm_twelve	12

verbose		2
loglevel	3
logfile		/home/me/rsnapshot.log
lockfile	/home/me/.pid/rsnapshot.pid

backup		/data/bigdump/	localhost/
{% endhighlight %}


And "crontab -e"

{% highlight bash %}
* */12 * * *   /usr/bin/rsnapshot rh_two
# every 12 hours, keep 2 rotating snapshots

50 23  * * *   /usr/bin/rsnapshot rd_seven
# back up daily at 23:50, keep 7 rotating snapshots (for a week)

40 23  * * 5   /usr/bin/rsnapshot rw_four
# back up weekly on Friday at 23:40, keep 4 rotating snapshots (for four weeks)

30 23  1 * *  /usr/bin/rsnapshot rm_twelve
# back up monthly on 1st of each month at 23:30, keep 12 rotating snapshots (four a year)
{% endhighlight %}


### 5. Advanced Usage

### Reference

* [Rsnapshot how-to](http://rsnapshot.org/howto/1.2/rsnapshot-HOWTO.en.html)

* [Post on AskUbuntu: "Backup intervals" in rsnapshot.conf](http://askubuntu.com/questions/20600/backup-intervals-in-rsnapshot-conf)
