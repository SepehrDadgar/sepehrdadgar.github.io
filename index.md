---
layout: page
title: Home
---

# Welcome to My Personal Website

Hi there! I'm {{ site.author }}, an Electronics Engineer passionate about technology, programming, and sharing knowledge.

مرا نه دولت وصل و نه احتمال فراق

نه پای رفتن از این ناحیت نه جای مُقام

___
... and then there is necessity and the needs of the whole world, of which you are a part of. whatever the nature of the whole does, and whatever serves to maintain it, it is good for every part of nature. the world is maintained by change--in elements and in the things they compose. that should be enough for you; treat it as an axiom. ... -Meditations Book 2, 3
## Latest Blog Posts

{% for post in site.posts limit:3 %}
- [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%B %-d, %Y" }}
{% endfor %}

[View all posts](/archive) | [View my projects](/projects)

## Get In Touch

Feel free to reach out to me through [email]({{ site.email }}) or connect with me on [GitHub](https://github.com/{{ site.github_username }}) and [LinkedIn](https://linkedin.com/in/{{ site.linkedin_username }}).

<style>
  .welcome-section {
    margin-bottom: 2rem;
  }
  
  .featured-posts {
    background: #f8f9fa;
    padding: 1.5rem;
    border-radius: 8px;
    margin: 2rem 0;
  }
  
  .featured-posts h2 {
    margin-top: 0;
    color: #2c3e50;
  }
  
  .cta-buttons {
    display: flex;
    gap: 1rem;
    margin: 2rem 0;
  }
  
  .cta-button {
    display: inline-block;
    padding: 0.8rem 1.5rem;
    background-color: #2a7ae2;
    color: white !important;
    text-decoration: none;
    border-radius: 4px;
    transition: background-color 0.2s;
  }
  
  .cta-button:hover {
    background-color: #1a5cb0;
  }
  
  @media (max-width: 600px) {
    .cta-buttons {
      flex-direction: column;
      gap: 0.5rem;
    }
    
    .cta-button {
      text-align: center;
    }
  }
</style>
