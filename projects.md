---
layout: page
title: Projects
permalink: /projects/
---

{% assign projects = site.projects | sort: "date" | reverse %}
<ul>
  {% for project in projects %}
  <li>
    <a href="{{ project.url | relative_url }}">{{ project.title }}</a>
    <span class="project-meta">- {{ project.date | date: "%B %Y" }}{% if project.tags.size > 0 %} &middot; {{ project.tags | join: ", " }}{% endif %}</span>
    {% if project.description %}<p class="project-desc">{{ project.description }}</p>{% endif %}
  </li>
  {% endfor %}
</ul>
