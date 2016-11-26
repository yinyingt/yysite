---
layout: post
title: Install Ruby and Jekyll in Linux with rvm
mathjax: false
related: false
comments: false
---

_Last updated in Feb 2015_


## Ruby Installation

Many Linux distributions provide Ruby packages in repositories, but I still find [RVM](http://rvm.io) is the best way of installing Ruby and managing its versions and packages. 

In my Ubuntu, first install dependencies (c compiler and curl)

{% highlight bash %}
sudo apt-get install gcc make curl
{% endhighlight %}

Second, install a few libraries

{% highlight bash %}
sudo apt-get install zlib1g-dev libreadline-dev libssl-dev libxml2-dev
{% endhighlight %}


__Update (Jan 2015)__: Add its GPG key of RVM as follows: 

{% highlight bash %}
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
{% endhighlight %}

Now install RVM 

{% highlight bash %}
\curl -sSL https://get.rvm.io | bash -s stable
{% endhighlight %}

and add those lines to '$HOME/.bashrc'

{% highlight bash %}
[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm"
{% endhighlight %}

which initialize RVM environment in each BASH session. And source the '.bashrc' file

{% highlight bash %}
. $HOME/.bashrc
{% endhighlight %}

Now it is time to install Ruby. But before that, check 

{% highlight bash %}
rvm requirements
{% endhighlight %}

and it gives you all the packages needed for Ruby installation. Install them before proceeding. 

At last, install the Ruby. At the time of writing, the newest Ruby stable version is 2.1.2, so type: 

{% highlight bash %}
rvm install 2.1.2
{% endhighlight %}

The latest stable Ruby version can be found [here](https://www.ruby-lang.org/en/downloads/). 


## Jekyll Installation 

[Jekyll](http://jekyllrb.com) is a Ruby gem. So it can be installed with `gem`. 

{% highlight bash %}
gem install rdoc  # refer to https://github.com/github/pages-gem/issues/9
gem install jekyll
{% endhighlight %}

**Update (Jun, 2014)**: Currently Jekyll requires CoffeeScript gem even if it is not used, which further requires JavaScript runtime. It can be installed with therubyracer gem like this 

```
sudo apt-get install g++
gem install therubyracer
```

__Updated: Aug 2016__

The new Jekyll simplify the process, and the build can be automated by the Gemfile and bundle. See [Github doc here](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll) for more information. 

To run Jekyll with a public IP, do 

```
bundle exec jekyll serve --host=0.0.0.0
```

The "--host=0.0.0.0" will bind server to all IP address rather than the localhost by default. 

Jekyll tutorials can be found in [Jekyll's website](http://jekyllrb.com/).
