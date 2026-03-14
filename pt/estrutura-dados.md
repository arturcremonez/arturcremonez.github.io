---
layout: page
title: "Estrutura de Dados"
permalink: /pt/estrutura-dados/
---

---
### Notas e Artigos

{% assign posts = site.projects_pt | sort: "order" %}
{% for post in posts %}
  {% if post.category == "data-structures" or post.category == "estrutura-dados" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---
