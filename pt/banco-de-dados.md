---
layout: page
title: "Banco de Dados"
permalink: /pt/banco-de-dados/
---

---
### Notas e Artigos

{% assign posts = site.projects_pt | sort: "order" %}
{% for post in posts %}
  {% if post.category == "database" or post.category == "banco-de-dados" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---
