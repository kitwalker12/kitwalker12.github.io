---
layout: post
title: Controller Polling Worker in Rails
date: 2014-10-06 18:00:00 -07:00
comments: true
categories: coding web
tags: code ruby controller worker poll
---

A lot of my time goes into making reports relating to eCommerce and with new data, some reports which are the results of interleaving a lot of tables tend to get really large and expensive in computation times. Here, clever controllers or optimized models wouldn't just cut it. If you face such a situation where attempts at refactoring for performance have been exhausted, you can have the output computed in a background worker using [```Sidekiq```](https://github.com/mperham/sidekiq) and a nifty extension called [```Sidekiq-Status```](https://github.com/utgarda/sidekiq-status). What I do is simply create the job in the cotroller using the parameters supplied and using jquery calls, just poll the controller till the job finishes or fails. Using Sidekiq-status module's DSL, we can store the related output of the worker in Redis and retreive it in the controller.

Here's the [code](https://gist.github.com/kitwalker12/513d55721b787a160426):

{% gist 513d55721b787a160426 %}
