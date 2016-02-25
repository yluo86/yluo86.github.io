---
layout: post
title:  "Pipeline for genotype array design"
date:   2016-01-12 15:11:17
categories: science
tags: GWAS pipeline bioinformatics
author: Yang
permalink: array-design
---

Recently in my work, I need to undergo a genome-wide association study in Peruvian population. Since most of the commercial genotype arrays are designed targeting primarily at European population. If you are also interested in either evaluating the coverage of the current commercial array for a particular dataset/population, or putting customized content in an existing chip specific to a population in order to improve coverage, this might a helpful post for you.

# What to put on the chip
1. Preselect set
    1. Commercial array contents: this can be any commercially available arrays, e.g. Affy6.0, Axiom, Illumina2.5, etc.
    2. Markers of interest: this can be signals from GWAS studies, HLA regions, eQTL or any other sources of interest.

2. Additional content
    1. Aims
        1. include as many variants as possible based on
            * Low-coverage whole genome sequencing data (1000GP)
            * Exome-sequencing data (from own dataset)
        2. r^2 > 0.8


  You can have properly indented paragraphs within list items. Notice the blank line above, and the leading spaces (at least one, but we'll use three here to also align the raw Markdown).

  To have a line break without a paragraph, you will need to use two trailing spaces.⋅⋅
  Note that this line is separate, but within the same paragraph.⋅⋅
   (This is contrary to the typical GFM line break behaviour, where trailing spaces are not required.)
