---
layout: post
title: Finally a CORS config that works on S3
date: 2014-10-21 13:00:00 -07:00
comments: true
categories: coding web
tags: cors s3 config
---

Something about how Google handles CORS changed in the recent update and the policies are somehow more restrictive than before if you use fonts hosted on S3.

The issue arises because of security concerns related to loading fonts from an origin different from the host origin. Many rails applications are deployed with their assets hosted on S3 and sometimes via a CDN which redirects a URL like cdn.your-domain.com to your-bucket.s3.amazonaws.com.

In such cases, the solution is to edit your S3 CORS confiuration to reflect the list of allowed origins which are trusted by you.

Here is a gist for the CORS config we gleaned from [this stackoverflow Question](http://stackoverflow.com/questions/12229844/amazon-s3-cors-cross-origin-resource-sharing-and-firefox-cross-domain-font-loa/12330829#12330829).

{% gist a4c066f9d2daef4544d7 %}
