---
layout: page
title: "Discrete Mathematics"
permalink: /discrete-mathematics/
---

---
### Notes and Articles

{% assign posts = site.projects_en | sort: "order" %}
{% for post in posts %}
  {% if post.categories contains "discrete-math" or post.category == "discrete-math" or post.categories contains "matematica-discreta-finita" or post.category == "discrete-mathematics" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---
