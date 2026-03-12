---
layout: page
title: "Compiladores"
permalink: /pt/compiladores/
---

---
### Notas e Artigos

{% assign posts = site.projects_pt | sort: "order" %}
{% for post in posts %}
  {% if post.category == "compiladores" or post.category == "compilers" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---
