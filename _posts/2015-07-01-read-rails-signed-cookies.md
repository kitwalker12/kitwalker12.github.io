---
layout: post
title: How to read signed Cookies in Rails
date: 2015-07-01 11:30:00 -07:00
comments: true
categories: coding
tags: rails cookies secure
---

In a rails controller or a helper you can easily read a signed cookie via `cookie.signed[:<cookie_name>]`.
Outside that, if you have access to your rails server (which has the secret key), you can use Rack to read the secure cookie string. The default Rails 3/4 session store (cookie store) allows users to read the contents of the session. They may not, however, change or tamper with the session data.

Follow this gist to read a signed cookie/session string.

{% gist 969e503d33f95d5aaf01 %}
