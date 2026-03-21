---
layout: page
title: "Matemática Discreta e Finita"
permalink: /pt/matematica-discreta-finita/
---

---
### Notas e Artigos

{% assign posts = site.projects_pt | sort: "order" %}
{% for post in posts %}
  {% if post.category == "matematica-discreta-finita" or post.categories contains "matematica-discreta-finita" or post.category == "discrete-math" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---
