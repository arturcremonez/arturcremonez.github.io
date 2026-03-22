---
layout: page
title: "Data Structures"
permalink: /data-structures/
---

---
### Notes and Articles

{% assign posts = site.projects_en | sort: "order" %}
{% for post in posts %}
  {% if post.categories contains "data-structures" or post.category == "data-structures" or post.categories contains "estrutura-dados" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---
