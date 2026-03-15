---
layout: page
title: "Computer Architecture"
permalink: /computer-architecture/
---

---
### Notes and Articles

{% assign posts = site.projects_en | sort: "order" %}
{% for post in posts %}
  {% if post.category == "computer-architecture" or post.category == "arquitetura-computadores" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---
