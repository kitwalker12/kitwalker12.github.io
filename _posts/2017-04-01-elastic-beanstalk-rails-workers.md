---
layout: post
title: Run Rails + Sidekiq + Sneakers on AWS Elastic Beanstalk
date: 2017-04-01 10:51:00 -07:00
comments: true
categories: coding
tags: rails sidekiq sneakers rabbitmq aws beanstalk passenger
---

*Here's a small primer to setup some workers with Rails on AWS Elastic Beanstalk*
*This assumes you've already setup a rails app on AWS. [Here's a good resource to get started if you haven't](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_Ruby_rails.html)*

# Sidekiq

You'll need 3 files in your `.ebextensions`. One to create the Upstart service, one to reload service configuration and one for hooks that start/stop the worker on beanstalk deploy hooks. Ofcourse these configurations can be added to a single file but I prefer to keep them separated for manageability.
See complete gist after the Sneakers description.

# Sneakers

If your app used sneakers to listen on RabbitMQ exchanges, you can also start sneakers workers in the background just like sidekiq. The configuration is a bit different though because sneakers doesn't provide a pid file using which we stop or start the process. Even if we kill the sneakers service there are orphan processes still running which were started by the sneakers rake task. I chose to kill those processes on redeploy.
See complete gist:


{% gist 0313e0edd66e5bfa7aed78f6890fab9d %}
