---
layout: category
title: "html"
category: "html"
permalink: /categories/html/
---


{% for post in site.categories.HTML reversed %}
- [{{ post.title }}]({{ post.url }}) – {{ post.date | date: "%Y-%m-%d" }}
{% endfor %}