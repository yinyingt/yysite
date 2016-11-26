---
layout: post
title: "Set Up Email Service With Custom Domain For Free"
mathjax: false
related: false
comments: true
published: true
---


_Created: Mar, 2016_

__NOTE: There is legitmate concern on Email privacy for both free and paid Email hosting services, which is beyond the scope of this post. Use at your own discretion.__

At the time of writing (2016), Zoho and Yandex provide free Email hosting with custom domain names more "generously" than others. Zoho is designed more for companies since Zoho Mail is part of Zoho enterprise suite. But one thing to like Yandex is that it provides mail aliases. 


## Zoho

The process is well documented in [Zoho's page](https://www.zoho.com/mail/help/email-hosting-with-zoho.html). Zoho offers 25 email addresses with one domain name for free. However, only old-schooled (e.g., .com, .net., .org, etc.) TLDs are supported. 


## Yandex

Yandex offer 1000 email for one domain name, and you can host several domain names under a single Yandex account. 

* Get a domain, and delegate the DNS administration to CloudFlare (or some other DNS service you like). One can even delegate the subdomain DNS administrations to a similar service such as dns.he.net. 

* Register a common Yandex account, which will be the administrator account for the services related to your domains. Go to [Yandex Domain](https://domain.yandex.com/), add your domain(s). Verify and create MX records by following the instructions there. You can also set up mail services for subdomains since Yandex Domain service is not restricted to top-level domains as Zoho. 

* Create emails account under that domain. Log into that account, and finish the initialization. 

* To access emails on phones and tablets, there are two ways. One is to use Yandex Mail app (straightforward). The other is to [use an email client](https://yandex.com/support/mail/mail-clients.xml#imap). The catch, which is not documented in Yandex FAQ pages, is that the main password won't work with 3rd party email clients at the time of writing (although the FAQ pages said it should work). One needs to first enable application-specific password and use that instead. I guess the reason is mainly about security concerns. It is a good sign Yandex takes measures like this.
