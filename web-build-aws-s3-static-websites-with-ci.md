---
layout: post
title: "How to Build a Static Website Hosted on AWS S3 with Continous Integration"
mathjax: false
related: false
comments: true
published: true
---


_Created: Apr, 2016_



This tutorial shows the work flow of building a HTTP-only static website hosted on AWS S3, with the option of using a continous integration (CI) service like [Wercker](http://wercker.com/). The website content will be directly sourced from a AWS S3 bucket. One can also use [Amazon CloudFront](http://aws.amazon.com/cloudfront/) for content delivery, which is decribed in [this post](./web-build-aws-s3-static-websites-https-enabled.html).


## Preparation

Obviously, you need to register for an AWS account if you haven't. AWS provides a bunch of services and the number is growing, but below is a subset for this tutorial:

* [Amazon Simple Storage Service (S3)](https://aws.amazon.com/s3/) for storing your website content;
* [Amazon Route 53](https://aws.amazon.com/route53/) for managing DNS of your domain;
* [AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/) for managing user access;
* (Optional) [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) for monitoring AWS resource usage. 

Here I build a static website with the domain name "example.com". 


## Step 1: Set Up AWS S3 and Route 53

The idea is to create two S3 buckets, with one named "example.com" hosting the website content and the other being "www.example.com" with no content which redirects to "example.com". It is suggested to name the S3 buckets with names like "example.com" above because it is will be a unique AWS-wide identifier. 

Pricing-wise, you will be charged with both storage and traffic on S3 (see [S3 pricing](https://aws.amazon.com/s3/pricing/)). 

Follow [this example](http://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html) in AWS documentation on setting up a simple static website with custom domain. The steps are: 

* Create buckets "example.com" and "www.example.com".
* Upload a test index page "index.html" to the root directory of bucket "example.com". 
* Enable website hosting on bucket "example.com", configure the bucket policy, and set the index document to "index.html". Till now, the website is viewable with the URL like `http://example.com.s3-website-us-east-1.amazonaws.com`.
* Enable request redirection on bucket "www.example.com" and redirect it to "example.com".
* Log in Route 53 console, and create a "Hosted Zone" for domain example.com. 
* Add an "alias" record of "example.com" and route it to the corresponding S3 endpoint in the drop-down menu. Note that "alias" (or called "ANAME" by others) record of an apex domain name is "CNAME"-like, but it returns A record because apex domain cannot have CNAME record. 
* Delegate domain "example.com" to Route 53 by setting the DNS servers of "example.com" to the ones provided by Route 53. 
* Wait for a few minutes and let the DNS change propagate. If everything works, the website can be visited with URLs like "http://example.com", "http://www.example.com", "http://example.com.s3-website-us-east-1.amazonaws.com" and "http://www.example.com.s3-website-us-east-1.amazonaws.com". All of those URLs will be (redirected to) "http://example.com" in the end. 


## Step 2: Configure Access in AWS IAM

Follow [this example](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSGettingStartedGuide/AWSCredentials.html) to get an AWS access key ID and key secret pair. After that, attach an IAM policy to grant user the access to the corresponding S3 bucket. For convenience, I use "AmazonS3FullAccess" policy since it is an one-man project. But you may need to be more careful about the access assignment when there are multiple collaborators. 


## Step 3: Include Continous Integration Service

This part has been spinned off to another standalone post [here](./web-hugo-aws-s3-wercker.html). The goal is to reduce the process-specific overhead by automating the build and deploy so that you can concentrate on the content creation.



## Links

* [A brief introduction to DNS host types](https://www.zerigo.com/docs/managed-dns/host_types)
