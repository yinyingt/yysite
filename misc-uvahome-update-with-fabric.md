---
layout: post
published: true
title: Update UVA Personal Homepage Off-Ground with Fabric
mathjax: false
related: false
comments: false
---

Last updated: Sep, 2013

I used to update my UVA personal webpage via blue.unix.virginia.edu which could be accessed off-Ground. But it was retired in August 2013, and its successor fir.itc.virginia.edu could only be accessed on Ground. Here is how I update this webpage now:

* Find another computer running on Ground (a Raspberry Pi running in office)
* Generate a SSH public-key (with empty passphrase) and add it to fir
* Approach: localhost --> RPi on Ground --> fir.itc.virginia.edu

And a Python library called Fabric can be used to automate the process. Install Fabric (in Ubuntu) first: 

{% highlight bash %}
sudo apt-get install fabric
{% endhighlight %}

The fabfile can be found [in my Gist](https://gist.github.com/lijunhw/6479886). Save this file somewhere. Also, create a rcfile in **$HOME/.fabricrc** with content of 

{% highlight bash %}
fabfile = path_to_your_fabfile
{% endhighlight %}

To update the webpage, run 

{% highlight bash %}
fab fir_html
{% endhighlight %}
