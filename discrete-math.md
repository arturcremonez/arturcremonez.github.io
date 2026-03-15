---
layout: page
title: "Discrete Math"
permalink: /pt/discrete-math/
---

---
### Notes and Articles

{% assign posts = site.projects_en | sort: "order" %}
{% for post in posts %}
  {% if post.category == "discrete-math" or post.category == "matematica-discreta-finita" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---
