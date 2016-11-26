---
layout: post
title: Quick Reference of R
mathjax: false
related: false
comments: false
published: true
---


_Last modified: Jan, 2013_


## Install Precompiled R Binaries on Ubuntu

The instructions have been well documented at [this link](http://watson.nci.nih.gov/cran_mirror/bin/linux/ubuntu/README.html). 

## Compile R from source on Ubuntun

This is an alternative from installing the binaries from a repository.

Download R source tar ball from [http://r-project.org/](http://r-project.org/). R depends on several libraries. Install them first before compiling R. 

```
sudo apt-get install gcc gfortran libreadline-dev xorg-dev
```

Extract the tar ball and cd into the directory. Configure the installation 

```
./configure --prefix=$HOME/local
```

Here I want to install all compiled results to _$HOME/local_. After this command exits without error, do 

```
make
```

This is going to take a while. When it finishes, you can either link the 'bin/R' a place you like, or do

```
make install
```

to install the whole thing under _$HOME/local_.


## Find the dimension of a dataframe

* nrow(df) returns the number of row
* length(df) returns the number of columns
* dim(df) returns the dimension (like the size() function in Matlab)

## Import data to R

Most commonly: 

```
mydata <- read.table("c:/mydata.csv", header=TRUE, sep=",", row.names="id")
```

Reference: http://www.statmethods.net/input/importingdata.html

## Remove all variables in the workspace

```
rm(list=ls(all=TRUE))
```

like the clear() in Matlab