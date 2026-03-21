---
layout: page
title: "Análise e Projeto de Algoritmos"
permalink: /pt/analise-projeto-algoritmos/
---

---
### Notas e Artigos

{% assign posts = site.projects_pt | sort: "order" %}
{% for post in posts %}
  {% if post.category == "analise-projeto-algoritmos" or post.categories contains "analise-projeto-algoritmos" or post.category == "algorithm-design" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---
