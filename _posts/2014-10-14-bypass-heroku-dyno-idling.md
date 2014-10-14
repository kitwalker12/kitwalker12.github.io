---
layout: post
title: How to avoid Heroku Dyno Idling
date: 2014-10-14 14:00:00 -07:00
comments: true
categories: coding web
tags: code heroku newrelic
---

I deploy a lot of production apps on Heroku, but the dyno idling on single dyno apps is a real bummer for me for the staging environments I've setup. I don't recommend this method to bypass the restriction, but considering I pay for about 5 production apps with one of them being on PX dynos, I feel pretty justified about employing it in my case.

This is what you need to do:

    heroku addons:add newrelic:wayne -a myappname
    heroku addons:open newrelic -a myappname

You'll be directed to the new relic setup page. It asks you to the following things:

* Add ```gem 'newrelic_rpm'``` to your gemfile and run ```bundle install```.
* Download the generated ```newrelic.yml``` on the setup page and place it in the ```config``` folder in your app.
* deploy to heroku.
* Click 'Connect my app' on the setup page.
* Launch the dashboard for your app from the listed apps.

From here, you can go to 'Settings' in the sidebar and click 'Availability Monitoring'.
Enter your URL in the form and click 'Start Pinging' and you're all set to go.


