---
layout: page
title: "Banco de Dados"
permalink: /pt/banco-de-dados/
---

---
### Notas e Artigos

{% assign posts = site.projects_pt | sort: "order" %}
{% for post in posts %}
  {% if post.category == "banco-de-dados" or post.categories contains "banco-de-dados" or post.category == "database" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---
