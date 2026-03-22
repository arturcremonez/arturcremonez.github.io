---
layout: page
title: "Database"
permalink: /database/
---

---
### Notes and Articles

{% assign posts = site.projects_en | sort: "order" %}
{% for post in posts %}
  {% if post.categories contains "database" or post.category == "database" or post.categories contains "banco-de-dados" or post.category == "banco-de-dados" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---
