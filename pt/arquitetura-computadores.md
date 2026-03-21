---
layout: page
title: "Arquitetura de Computadores"
permalink: /pt/arquitetura-computadores/
redirect_from:
  - /pt/compiladores/
---

---
### Notas e Artigos

{% assign posts = site.projects_pt | sort: "order" %}
{% for post in posts %}
  {% if post.category == "arquitetura-computadores" or post.categories contains "arquitetura-computadores" or post.category == "computer-architecture" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---