---
layout: page
title: Projects
permalink: /projects/
---

<div class="projects">
  {% for project in site.projects %}
    <div class="project">
      <h2><a href="{{ project.url | relative_url }}">{{ project.title }}</a></h2>
      <p class="project-meta">
        <span class="project-date">{{ project.date | date: "%B %-d, %Y" }}</span>
        {% if project.tags.size > 0 %}
          <span class="project-tags">
            {% for tag in project.tags %}
              <span class="tag">{{ tag }}</span>
            {% endfor %}
          </span>
        {% endif %}
      </p>
      <p>{{ project.description }}</p>
      {% if project.github or project.live_demo %}
        <div class="project-links">
          {% if project.github %}
            <a href="https://github.com/{{ project.github }}" class="btn" target="_blank">
              <i class="fab fa-github"></i> View on GitHub
            </a>
          {% endif %}
          {% if project.live_demo %}
            <a href="{{ project.live_demo }}" class="btn" target="_blank">
              <i class="fas fa-external-link-alt"></i> Live Demo
            </a>
          {% endif %}
        </div>
      {% endif %}
    </div>
  {% endfor %}
</div>

<style>
.projects {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 2rem;
  margin: 2rem 0;
}

.project {
  padding: 1.5rem;
  border: 1px solid #e1e4e8;
  border-radius: 6px;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.project:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.project h2 {
  margin-top: 0;
  margin-bottom: 0.5rem;
}

.project h2 a {
  color: #24292e;
  text-decoration: none;
}

.project h2 a:hover {
  color: #0366d6;
}

.project-meta {
  color: #6a737d;
  font-size: 0.9em;
  margin-bottom: 1rem;
}

.project-tags {
  display: inline-flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-left: 0.5rem;
}

.tag {
  background-color: #f1f8ff;
  color: #0366d6;
  padding: 0.2rem 0.6rem;
  border-radius: 2em;
  font-size: 0.8em;
  white-space: nowrap;
}

.project-links {
  margin-top: 1rem;
  display: flex;
  gap: 1rem;
}

.btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem 1rem;
  background-color: #f6f8fa;
  color: #24292e;
  text-decoration: none;
  border: 1px solid rgba(27, 31, 35, 0.15);
  border-radius: 6px;
  font-size: 0.9em;
  transition: background-color 0.2s ease;
}

.btn:hover {
  background-color: #eaecef;
  text-decoration: none;
}

.btn i {
  font-size: 1.1em;
}
</style>
