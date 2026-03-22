---
layout: page
title: "Computer Architecture"
permalink: /computer-architecture/
redirect_from:
  - /compilers/
---

---
### Notes and Articles

{% assign posts = site.projects_en | sort: "order" %}
{% for post in posts %}
  {% if post.categories contains "computer-architecture" or post.category == "computer-architecture" or post.categories contains "arquitetura-computadores" or post.category == "arquitetura-computadores" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---
