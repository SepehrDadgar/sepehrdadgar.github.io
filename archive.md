---
layout: page
title: Blog Archive
hand_title: archive
---

{% assign posts_by_year = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
{% for year in posts_by_year %}
<h3>{{ year.name }}</h3>
<ul>
  {% for post in year.items %}
  <li><a href="{{ post.url | relative_url }}">{{ post.date | date: "%B %-d" }} - {{ post.title }}</a></li>
  {% endfor %}
</ul>
{% endfor %}
