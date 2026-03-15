---
layout: page
title: "Algorithm Design"
permalink: /algorithm-design/
---

---
### Notes and Articles

{% assign posts = site.projects_en | sort: "order" %}
{% for post in posts %}
  {% if post.category == "algorithm-design" or post.category == "analise-projeto-algoritmos" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---
