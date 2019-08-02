---
layout: post
title:  "Running Jupyter Lab on the (broad) server"
date:   2019-06-04
categories: snippets
author: Yang
permalink: jupyter-lab
---

# Running Jupyter Lab on the Broad Server

## Create an conda environment with customized location
{% highlight bash %}
conda create --prefix=/medpop/srlab/yang/condapkgs/py3 python=3.6
{% endhighlight %}

To activate this environment, use:
{% highlight bash %}
source activate /medpop/srlab/yang/condapkgs/py3
{% endhighlight %}
## installing jupyter lab
{% highlight bash %}
conda install -c conda-forge jupyterlab
{% endhighlight %}
## Installing IRkenrel
{% highlight bash %}
install.packages('IRkernel')
IRkernel::installspec()  # to register the kernel in the current
{% endhighlight %}
## to start
{% highlight bash %}
ish -l h_vmem=8g,os=RedHat7

jupyter lab
{% endhighlight %}
