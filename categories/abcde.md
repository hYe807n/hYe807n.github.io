---
layout: page
title: Abcde
permalink: /codereview/categories/abcde/
---

<h5>{{ page.title }}</h5>

<div class="card">
{% for post in site.categories.abcde %}
 <li class="category-posts"><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</div>