---
layout: default
title: Archive
---

# 게시글 모음

<!-- {% assign postsByYearMonth = site.posts | group_by_exp: "post", "post.date | date: '%B %Y'" %}
{% for yearMonth in postsByYearMonth %}
  <h2>{{ yearMonth.name }}</h2>
  <ul>
    {% for post in yearMonth.items %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %} -->


<!-- 카테고리별 정렬 -->
{% assign postsByCategory = site.posts | group_by: "category" %}

{% for category in postsByCategory %}
<hr>
## [ {{ category.name }} ]

{% assign postsByYearMonth = category.items | sort: "date" | group_by_exp: "post", "post.date | date: '%B %Y'" %}

{% for yearMonth in postsByYearMonth %}
<!-- ### &emsp; <i>{{ yearMonth.name }}</i> -->
{% for post in yearMonth.items %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}
{% endfor %}

{% endfor %}