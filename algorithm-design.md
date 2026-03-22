---
layout: page
title: "Algorithm Design"
permalink: /algorithm-design/
---

---
### Notes and Articles

{% assign posts = site.projects_en | sort: "order" %}
{% for post in posts %}
  {% if post.categories contains "algorithm-design" or post.category == "algorithm-design" or post.categories contains "analise-projeto-algoritmos" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---