---
layout: post
title: Enable HTTPS redirect for AWS Elastic Beanstalk with a Rails App behind a Load Balancer
date: 2017-04-02 10:51:00 -07:00
comments: true
categories: coding
tags: rails aws beanstalk passenger https
---

I had a bit of a difficulty with enabling https redirects on a Elastic Beanstalk deployment with a Rails Application behind a Load Balancer.

For a Rails app running behind passenger, the nginx config for the load balancer is generated from an ERB template, instead of a custom nginx config file you can just upload like many blogposts I saw about the issue suggested.

The file that needs to be overwritten can be found [here](https://github.com/awslabs/elastic-beanstalk-docs/blob/master/configuration-files/aws-provided/security-configuration/https-redirect-load-balanced-ruby-passenger/https-redirect-ruby-passenger.config). Amazon Support has provided a bunch of configuration files for many scenarioes so [this is a pretty helpful repository to follow](https://github.com/awslabs/elastic-beanstalk-docs).

We'll need to copy the whole file and put it under our `.ebextensions` folder. The code that performs the redirect is already included in that file as the first location directive that was added and is noted below. We'll be redirecting all requests to https, while retaining the parameters, except the requests that go to the health check URL.

{% highlight ruby %}
  set $redirect 0;
  location / {
      if ($http_x_forwarded_proto != "https") {
        set $redirect 1;
      }
      if ($http_user_agent ~* "ELB-HealthChecker") {
        set $redirect 0;
      }
      if ($redirect = 1) {
        return 301 https://$host$request_uri;
      }
      passenger_enabled on;
  }
{% endhighlight %}
