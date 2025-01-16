---
layout: category
title: "Java"
category: "Java"
permalink: /categories/java/
---


{% for post in site.categories.Java reversed %}
- [{{ post.title }}]({{ post.url }}) – {{ post.date | date: "%Y-%m-%d" }}
{% endfor %}