---
layout: post
title: Methods and Messages in Ruby
date: 2014-10-22 12:00:00 -07:00
comments: true
categories: coding
tags: ruby methods messages
---

TIL how javascript like callbacks to functions may be achived in regular ruby code from [this screencast on RubyTapas](http://www.rubytapas.com/episodes/11-Method-and-Message?filter=free).

In essence, a message is a name for a *responsibility* which an object may have, and a method is a _named, concrete piece of code_ that encodes one way a responsibilty may be fulfilled.

{% gist 7ca077b56dd5fd8915d6 %}
