---
layout: category
title: "Web"
category: "Web"
permalink: /categories/web/
---


{% for post in site.categories.Web reversed %}
- [{{ post.title }}]({{ post.url }}) – {{ post.date | date: "%Y-%m-%d" }}
{% endfor %}