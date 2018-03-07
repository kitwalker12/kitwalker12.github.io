---
layout: post
title: CircleCI 2.0 Config for Rails application deploying to AWS
date: 2018-03-07 00:00:00 -07:00
comments: true
---

With CircleCI forcing everyone to migrate to their 2.0 platform before August, I though to jump on the bandwagon well in advance. Here's the setup necessary to migrate an existing rails app to CircleCI 2.0. There's config added here to support postgres and redis databases if your app uses them. Additonaly, I've used the workflows feature to deploy the application to Elastic Beanstalk.

{% gist c8c34081c84f5addeb4745c651ecf747 %}
