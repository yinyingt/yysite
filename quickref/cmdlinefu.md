---
layout: post
title: Command-Line-Fu
mathjax: true
related: false
comments: true
published: true
---





__Last modified: Mar, 2016__

A collection of random but useful command lines. The OS is assumed to be Ubuntu. 


## Batch process images

To resize all images in one folder, first install imagemagick. 

```
sudo apt-get install imagemagick
```

Then in the folder, run bash script: 

```
for img in `ls *.JPG`
  do 
  mogrify -resize 50% $img
done
```

## Networking

About IPv6 related commands: at the time of writing (Mar, 2016) IPv6 is still in the adopting stage and not every server can talk in IPv6. 

* To ssh into a IPv6 only Linux box: 

```
ssh -6 user@3509:5600:02e4:0000:0000:0000:098e:2abf
```

which is very similar to the traditional way. 

* To ping IPv6 addresses/host names, do 

```
ping6 ipv6.google.com
```


## Directory and File Operations

* Show meta information of hidden files with "du" (reference [here](http://askubuntu.com/questions/356902/why-doesnt-this-show-the-hidden-files-folders/363681#363681)): 

```
du -sch .[!.]* * |sort -h
```

* Change text pattern in files recursively:

```
find path_to_folder -type f -print0 | xargs -0 sed -i 's/patternA/patternB/g'

```

See [this post](http://stackoverflow.com/a/1583282) for explanation.


* Compare the content of files in two directories recursively: 

```
diff -qr -x "*.o" -x "*.html" -x "*.bin" dir1 dir2
```

Here the "-x" flag excludes the files to be compared.

* A "live" view of a log file using tail

```
cat log.txt | tail -f
```
