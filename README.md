blag
====

byte me;

## To create new posts

* Create new file in [`_posts`](https://github.com/kitwalker12/kitwalker12.github.io/tree/master/_posts) folder.
* Filename should follow the template `YYYY-MM-DD-URL.md`
* The post file should contain the following as the top section
```
---
layout: post
title: My Title
date: YYYY-MM-DD HH:MM:SS -07:00
comments: true
---
```
* Add post content under this header in regular markdown
* To insert gists use this shortcode: `{% gist sha-code %}`
* To highlight code use this format:
```
{% highlight ruby %}
  # your code
{% endhighlight %}

```
* Run locally with `jekyll serve`
