---
layout: page
permalink: /publications/
title: publications & technical reports
nav_title: publications
description: Publications and technical reports in chronological order.
nav: true
nav_order: 2
---

{% include bib_search.liquid %}

<h2>publications</h2>
<div class="publications">
{% bibliography -q @article %}
</div>

<!-- <h2>Manuscripts Under Review</h2>
<div class="publications">
{% bibliography -q @unpublished %}
</div> -->

<h2>technical reports</h2>
<div class="publications">
{% bibliography -q @techreport %}
</div>


