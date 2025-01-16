---
layout: categories
title: "Categories"
permalink: /categories/
---

{% for category in site.categories %}
### {{ category[0] }}  <!-- 카테고리 이름 -->
<ul>
  {% for post in category[1] %}
  <li>
    <a href="{{ post.url }}">{{ post.title }}</a> – {{ post.date | date: "%Y-%m-%d" }}
  </li>
  {% endfor %}
</ul>
{% endfor %}