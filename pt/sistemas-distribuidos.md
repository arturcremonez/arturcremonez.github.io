---
layout: page
title: "Sistemas Distribuídos"
permalink: /pt/sistemas-distribuidos/
---

---
### Notas e Artigos

{% assign posts = site.projects_pt | sort: "order" %}
{% for post in posts %}
  {% if post.category == "sistemas-distribuidos" or post.categories contains "sistemas-distribuidos" or post.category == "distributed-systems" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---
