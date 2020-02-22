---
layout: page
title: Qarcoal
categories:
  - Research
tags:
  - python
  - code
---

One of my friends in the lab, [Marcus Fedarko](https://twitter.com/mwfedarko), developed a really cool tool called [Qurro](https://github.com/biocore/qurro). Qurro is really useful for visualizing feature rankings from tools like [songbird](https://github.com/biocore/songbird) or [DEICODE](https://github.com/biocore/DEICODE). Basically, you inpupt the feature rankings and can then visualize them in the context of your metadata.

One of its many features is searching features by feature metadata and plotting their log-ratios vs. metadata categories as boxplots. This is super useful for exploratory analysis, but I found that it was a bit inconvenient to run multiple log-ratios, as you have to have to export the data from the webpage each time. (Of course, Qurro wasn't intended for this purpose!)

What I did was implement a new function in Qurro called *Qarcoal*. Qarcoal allows the user to compute the log-ratio of features matching a numerator string to those matching a denominator string. This can be done from the command line, so it doesn't require running songbird or DEICODE beforehand. Additionally, Qarcoal outputs the log-ratios in a format that functions as a valid Qiime2 Metadata file.

The primary use cases of Qarcoal I see are:

* Interrogating the same log-ratio(s) for multiple datasets
* Easily calculating the log-ratios for multiple sets of features from the command line/Artifact API
* Computing log-ratios of feature tables with entries on the scale of 10 quadrillion (seriously... Qurro can't do this because of JavaScript limitations)

See the Qurro pre-print on [bioRxiv](https://www.biorxiv.org/content/10.1101/2019.12.17.880047v1)!
