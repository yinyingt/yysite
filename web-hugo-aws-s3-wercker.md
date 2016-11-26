---
layout: post
title: "Build and Deploy Automation of Hugo Static Websites to AWS S3 with Wercker CI Service"
mathjax: false
related: false
comments: true
published: true
---


_Created: Apr, 2016_


This tutorial also applies to building and deploying static websites to AWS S3 in general with minor changes. 


## Introduction

Motivation: 

[Hugo](http://gohugo.io/) is a fantastic static website generator. [AWS S3](https://aws.amazon.com/s3/) can be a top-notch solution of serving static websites. However, building such a Hugo site and deploying it to AWS S3 needs some non-trivial steps with certain toolchains. Here, continous integration (CI) service comes in to save. Fortunately, many high-quality CI services are free to use with basic-tier plans, and [Wercker](http://wercker.com/) is one of them. 

Goal: 

By the end of this tutorial, you should be able to push your Hugo source to a remote git repository (Github or Bitbucket for now), and your website hosted on AWS S3 should be automated updated with the latest content. 

Prerequites:

* You have created an AWS S3 bucket, and also AWS IAM credentials (key ID and key secret) that can access this bucket. 
* You have registered an account on [Wercker](http://wercker.com)
* You have a Github or Bitbucket account



## Idea

The idea is simple:

![AWS S3 + Wercker CI](http://i.imgur.com/bc792TO.png?1)

* You have a remote git repository on Github or/and Bitbucket, which will be configured to allow the access from Wercker server. 
* Every time you push a new commit to the git repository, Github or Bitbucket will notify Wercker about the update. Wercker will then pull the code from your git repository, and launch a build job within a container (e.g., Docker) using Hugo tool. 
* After the build job is finished, you can trigger a deploy job to sync content to AWS S3. This step can also be automatically triggered if Wercker is configured with automatic deploy option (good for developing-stage iterations).

This is how "continuous integration" works in general. 


## Set up AWS 

Two things need to be done: 

* Create a user in [AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/), log down its credentials. 
* Create an IAM policy to grant it the access to the corresponding Amazon S3 bucket where the website is to be deployed to. I choose "AmazonS3FullAccess" for convenience. 


## Set up Wercker


The Wercker automation runs like a [multi-stage pipeline](http://devcenter.wercker.com/learn/pipelines/available-pipelines.html): dev -> build -> deploy. But we only use "build" and "deploy" stages here. This pipeline is decribed in the "wercker.yml" YAML file in the root directory. So create such a "wercker.yml" file with content like 

<div>
<script src="https://gist.github.com/lijunhw/9c0dd64024e90baa9deaebd86be9e368.js?file=wercker.yml"></script>
</div>

The above file is divided to two parts: 

* Build: It will take a template container called "[arjen/hugo-build](https://app.wercker.com/#applications/54a7744c6b3ba8733de4dcde)" in Wercker's container registry, and use it as the base for building a Hugo project. 
* Deploy: It will take the results from the build step, and sync it to a AWS S3 bucket. Note that the AWS IAM key ID and secret are specified with environmental variables, which will be configured later in Wercker. Avoid hard coding those sensitive information in plain text in your repository. 

More s3sync options can be found [here](https://github.com/wercker/step-s3sync). 

Now add, commit and push the code to the remote repository. 

Sign in to Wercker (or sign up if you haven't), and click "`Create`" -> "`Application`" to create a new Wercker app. This process will connect Wercker with your Github or Bitbucket where the Hugo source resides in, and also configure the access to the repository. For a public repository, Wercker will just pull via https every time. But if the repository is private, Wercker will automatically add its SSH public key to your Github or Bitbucket account. Not quite a hassle -- just follow the instructions. 

It is time to specify the value of the environmental variables in `wercker.yml` in Wercker as follows.

![Wercker GUI](http://i.imgur.com/Ga46RIj.png?1)

Clike "`Application`" tab in the top -> click on the Wercker app you just created -> click on "`Settings`" in the upper right corner. Now do two things: 

* In the left panel, click on "`Environmental variables`", and add `AWS_ACCESS_S3_KEY_ID` and `AWS_ACCESS_S3_KEY_SECRET` environmental variables with corresponding values. Be sure to check the "`protect`" boxes so the sensitive information is protected. 
* In the left panel, click on "`Targets`", and add a deploy target. The deploy target name can be anything meaningful such as "production", "release", or "development". If automatic deploy is needed, check the "Auto deploy" box, and specify the git branch that trigger(s) such a deploy. Below that, create an environmental "`AWS_S3_BUCKET_URL`" with valid value, which is generally in form of "`s3://example.com`".

One can see environmental variables can be set in both. The difference is the former is application-wide, while the latter is restricted to the deploy stage. 



## Action

Go to the Wercker app page, and trigger a build job. After it is done, trigger a deploy job to a target if automatic deploy is not set. You will also trigger such a event chain by pushing a new commit to the git repository on Github or Bitbucket. 



## Conclusion

This is awesome. You should try it. 


## References

* [Hugo documentation: automated deployment](http://gohugo.io/tutorials/automated-deployments/): this gives an example of automated deployment to Github Pages
* [Generate and publish Bitbucket repository to AWS S3](http://manuelgruber.com/2014/jekyll-bitbucket-to-aws-s3/): this tutorial works with an older version of Wercker.
* [wercker/step-s3sync](https://github.com/wercker/step-s3sync)
