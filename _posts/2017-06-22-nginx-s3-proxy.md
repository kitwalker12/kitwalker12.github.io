---
layout: post
title: Nginx Proxy for Maintenance Page hosted on S3
date: 2017-06-22 00:00:00 -07:00
comments: true
---

I needed to setup a maintenance page for a website which was deployed on elastic beanstalk.
As beanstalk doesn't support a maintenance mode, it fell upon me to create a server to which I can redirect all requests which EB maintenance is going on.

I already had a maintenance page designed and uploaded to S3 as a static website but I needed to make sure all requests are captured by this page while the main server is down. To this end, I setup an nginx proxy which would pass all requests to this static page hosted on S3.

In the nginx conf I made sure that all requests going to my-domain.com would get redirected to the maintenance page on S3. Also, as the assets (images/css/js) were stored in a folder next to the html file, those requests would need to be directed to the S3 asset path. I also added a directive to redirect to the nginx error page instead of the S3 Access Denied page in case a forbidden asset was accessed.

[Here is the conf file](https://gist.github.com/kitwalker12/acb51edce10481a375d409175fbb31c3):

{% gist acb51edce10481a375d409175fbb31c3 %}
