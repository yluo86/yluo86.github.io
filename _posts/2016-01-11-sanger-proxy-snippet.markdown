---
layout: post
title:  "ssh web-proxy -- working outside work"
date:   2016-01-11
categories: snippets
author: Yang
permalink: web-proxy
---

I recently moved to Boston from Sanger Institute, and had to access an internal website from a new laptop. This is the snippets of how to set it up, and generally speaking, it should work for any web proxy as long as you have a valid ssh login.

# 1. ssh from your terminal window:
{% highlight bash %}
ssh -L3128:cache1a.internal.sanger.ac.uk:3128 USERNAME@ssh.sanger.ac.uk
{% endhighlight %}

# 2. Change the proxy settings in the web browser to, the figure below shows a screen shot of my setting in Firefox

<img src="{{ site.baseurl }}/assets/images/firefox_proxy_setting.png" alt="firefox_proxy_setting" width=50%>

# Feeling lazy..
After doing this for a while, instead of typing the ssh command ever time, I created a tunnel set up in my config file `~/.ssh/config` and save some typing effort
{% highlight bash %}
Host Sanger_web
Hostname ssh.sanger.ac.uk
User USERNAME
LocalForward localhost:3128 cache1a.internal.sanger.ac.uk:3128
ForwardX11 yes
ForwardX11Trusted yes
{% endhighlight %}

And now I only need to do `ssh Sanger_web` to "jump the gate".
