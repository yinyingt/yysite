---
layout: post
title: GitHub Related
mathjax: false
related: false
comments: true
pulished: true
---

_Created: Mar, 2016_


Quick notes on using GitHub (not about git commands). 


* [Host static website on Github using GitHub Pages](https://help.github.com/categories/github-pages-basics/)

* [How to embed a specific file in a Gist repo](http://stackoverflow.com/a/14267385). This is a handy trick to embed code in posts. 


## Create Github Pages

Markdown cheatsheet can be found here https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet. 

Here is how to create a web page with custom domain on Github Pages: 

1. Follow this link https://help.github.com/articles/creating-project-pages-manually to create a Github page. The page can be accessed via
http://user_name.github.io/repository_name

2. Create a file "CNAME" under this "gh-pages" branch, and with the subdomain as the content, like
subdomain.example.com

3. Add a subdomain CNAME DNS record, pointing to "user_name.github.io". For top-level domain, A records should be created instead (not CNAME). In this case, consult [this link](https://help.github.com/articles/tips-for-configuring-an-a-record-with-your-dns-provider). 


4. Wait for a few minutes, and check http://subdomain.example.com. Note that anything under the "gh-pages" branch can be accessed like 
http://subdomain.example.com/something.html

5. Optional: add custom 404 page (https://help.github.com/articles/custom-404-pages) and use Jekyll (https://help.github.com/articles/using-jekyll-with-pages).