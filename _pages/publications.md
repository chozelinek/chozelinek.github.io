---
title: My Publications
permalink: /publications/
---

Download my publications list in [PDF]({{ site.url }}/assets/publications.pdf).

## Articles in journals

{% bibliography -q @article %}

## Conference proceedings

{% bibliography -q @inproceedings[keywords!=nopub] %}

## Contributions to edited volumes

{% bibliography -q @incollection %}

## Communications

{% bibliography -q @inproceedings[keywords=nopub] %}
