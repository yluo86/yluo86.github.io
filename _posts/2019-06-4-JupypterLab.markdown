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
conda create --prefix=/medpop/srlab/yang/condapkgs/py3 python=3.6

To activate this environment, use:

source activate /medpop/srlab/yang/condapkgs/py3

## installing jupyter lab
conda install -c conda-forge jupyterlab

#3 Installing IRkenrel
install.packages('IRkernel')
IRkernel::installspec()  # to register the kernel in the current

## to start
ish -l h_vmem=8g,os=RedHat7

jupyter lab
