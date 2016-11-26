---
layout: post
title: "How to Build a HTTPS-Enabled Static Website Hosted by AWS S3 + CloudFront"
mathjax: false
related: false
comments: true
published: true
---


_Created: Apr, 2016_



This tutorial shows the work flow of building a HTTPS-enabled static website hosted on AWS S3, with the option of using a continous integration (CI) service like [Wercker](http://wercker.com/). Different from a [previous post](./web-build-aws-s3-static-websites-with-ci.html), I need to use [Amazon CloudFront](https://aws.amazon.com/cloudfront/) for content delivery. 


## Motivation

// Beginning of TL;DR

There may be a few questions to be answered before you are convinced that this is a tutorial for you.

The first question is why I should have a HTTPS-enabled static website in the first place. While a static website has much less potential security issues compared with a dynamic site (which is one of the reasons behind the revival of static websites in recent years), I give two major benefits:

* It is a code fact that a HTTP static website is still largely vunerable from man-in-the-middle (MITM) attack for obvious reasons. For example, if your webpages run some 3rd-party HTTP requests, the results can be tampered.  
* Modern search engines like Google and Bing rank HTTPS URLs higher than HTTP counterparts. Search engine optimization (SEO) is a strong motivation to go for HTTPS. 

The second is why to pay the money and host on AWS S3 instead of simply hosting with a free service like [Github Pages](https://pages.github.com/). There is nothing wrong with Github Pages which I loved and used (and still do) for a long time. Github Pages has the advantage of being much easier to bring it up and working compared to the AWS S3 solution. _In fact this site is still hosted on Github Pages._ But there are several things to notice:

* At least at the moment of writing (Apr, 2016), Github Pages doesn't support HTTPS for custom domains (but it is available for sites like user.github.io). 
* AWS solution is better in performance. AWS S3 + Cloudfront is a top-notch solution for hosting static websites backed by global CDN. 
* The Github repository needs to be public so that Github Pages can be used. AWS S3 can be integrated with private repositories. This is something to consider if source privacy is a factor to weigh in. 
* AWS S3 is much more customizable. You can adjust many parameters such as cache timeout, page redirect, etc. 
* The SSL market landscape is changing -- there is strong industry-level push towards lowering the barrier of HTTPS. Amazon (or to be specific, Amazon Trust Services), has become a root [certificate authority (CA)](https://en.wikipedia.org/wiki/Certificate_authority) in 2016, and offer free SSL certificates with automatic renewal as long as you use AWS services (see [this post](https://aws.amazon.com/blogs/aws/new-aws-certificate-manager-deploy-ssltls-based-apps-on-aws/)). Another option is [Let's Encrypt](https://letsencrypt.org/) which also offer SSL certificates for free, but you need to take care of the provision yourself. 

The third is why to use a CI service. This one is obvious: the process chain to be described below is still non-trivial, and use a CI service like Wercker can save lots of process overhead and make you focus on the content in long run. In some sense, CI service is a must for AWS S3 static website hosting. 

To conclude, there are pros and cons on both sides. Make the decision based on your needs. 

// End of TL;DR



## Preparation

Obviously, you need to register for an AWS account if you haven't. AWS provides a bunch of services and the number is growing, but below is a subset for this tutorial:

* [AWS Certificate Manager](https://aws.amazon.com/certificate-manager/) for managing SSL certificate;
* [Amazon Simple Storage Service (S3)](https://aws.amazon.com/s3/) for storing your website content;
* [Amazon Route 53](https://aws.amazon.com/route53/) for managing DNS of your domain;
* [Amazon CloudFront](https://aws.amazon.com/cloudfront/) for content delivery with HTTPS
* [AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/) for managing user access;
* (Optional) [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) for monitoring AWS resource usage. 


Here I build a static website with the domain name "example.com". 


## Step 1: Set Up An AWS S3 Bucket

Some part of the this step is similar to the instructions in [this example](http://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html). The difference is the use of Route 53 is optional. 

Pricing-wise, you will be charged for [storage in S3](https://aws.amazon.com/s3/pricing/) and traffic from [CloudFront](https://aws.amazon.com/cloudfront/pricing/). This pricing model favors sites with high traffic because HTTP requests are much cheaper for CloudFront than S3. 

The steps are: 

* Create buckets "example.com" similar to the [previous post](./web-build-aws-s3-static-websites-with-ci.html).

* Upload a test index page "index.html" to the root directory of bucket "example.com". 

* Add bucket policy 

```
{
  "Version":"2012-10-17",
  "Statement":[{
	"Sid":"PublicReadGetObject",
        "Effect":"Allow",
	  "Principal": "*",
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::example.com/*"
      ]
    }
  ]
}
```

It is optional to enable website hosting for the S3 bucket, but it is still a good idea to do so in some circumstances (see Step 2). 



## Step 2: Obtain An SSL Certificate


Log in [AWS Certificate Manager (ACM)](https://aws.amazon.com/certificate-manager/) to request a SSL certificate as described in [this post](https://aws.amazon.com/blogs/aws/new-aws-certificate-manager-deploy-ssltls-based-apps-on-aws/). Amazon will send email to the administrative email address in the whois information of example.com, and to admin@example.com for domain owernship verification. So make sure at least one of these emails works. 

I usually request a SSL certificate for the "naked" domain `example.com`, and the first-level wildcard subdomains `*.example.com`. The SSL certificate can be quickly granted. 

After the SSL certificate is granted, log in the CloudFront and add a web distribution. In the "SSL certificate" section, select "Custom SSL Certificate" and select the SSL certifcate just created in ACM. And set "Default Root Object" to "index.html". Also, set viewer protocol policy as "redirect HTTP to HTTPS" to enforce SSL connection. 

At last, enable this distribution, and wait for a while as the files being deployed to different CDN data centers, till the status turns "Deployed". While waiting, proceed to the next step. 


## Step 3: Configure DNS and CloudFront

There are many ways:

* Use Route 53. After delegating "example.com" to Route 53, create an A record of "@" with alias pointing to the CloudFront endpoint. And then create a CNAME record of "www" pointing to "example.com". The result is, all of `https://example.com/`, `https://www.example.com/`, `http://example.com/` and `http://www.example.com/` will be visitable as is. 

* Or, use custom DNS. Create a CNAME record "www" and point it to the cloudfront URL (something like abcde12345.cloudfront.net). Set a redirect rule to redirect `example.com` to `www.example.com`. The problem with this approach is `https://example.com/` can not be correctly redirected to `https://www.example.com/`.

However, there is a caveat about CloudFront CDN service: CloudFront cannot resolve the default root object of subdirectories as `index.html`. For example, if there is a webpage at `https://www.example.com/project/index.html`, this link `https://www.example.com/project/` will return "access denied" error. To circumvent this quirk, enable web hosting on the source S3 bucket, and use its full URL (something like `www.example.com.s3-website-us-east-1.amazonaws.com`) as the origin. In that way, visiting `https://www.example.com/project/` will actually go to `www.example.com.s3-website-us-east-1.amazonaws.com/project/` first and fetch the index.html (since S3 support the default root object resolving of index.html). However, to some degree, this reduce the effectiveness of using a CloudFront CDN. But this is a work-around for AWS S3 + CloudFront solution. 

Note that [CloudFront supports GZip compression](https://aws.amazon.com/blogs/aws/new-gzip-compression-support-for-amazon-cloudfront/), so don't forget to enable it for better loading speed. 


## Step 4: Configure Access in AWS IAM & Include CI Service

The above steps should already give you a working HTTPS-enabled website. This step is about continuous integration which makes developing and deploying the website easier. This step is identical to Step 2 and Step 3 in the [previous post](./web-build-aws-s3-static-websites-with-ci.html).
