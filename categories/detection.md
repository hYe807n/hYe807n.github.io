---
layout: page
title: Detection
permalink: /paperreview/categories/detection/
---

<h5>{{ page.title }}</h5>

<div class="card">
{% for post in site.categories.detection %}
 <li class="category-posts"><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</div>