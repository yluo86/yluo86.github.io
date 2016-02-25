---
layout: post
title:  "Recursive touch -- giving a new timestamp to your files"
date:   2015-09-11
categories: snippets
author: Yang
permalink: recursive-touch
---

If you ever need to change the timestamp of a whole directory, here is a one liner to do so:

{% highlight bash %}
find . -exec touch {} \;
{% endhighlight %}
