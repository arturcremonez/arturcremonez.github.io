---
layout: page
title: "Discrete Mathematics"
permalink: /discrete-mathematics/
---

---
### Notes and Articles

{% assign posts = site.projects_en | sort: "order" %}
{% for post in posts %}
  {% if post.category == "discrete-mathematics" or post.category == "matematica-discreta-finita" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---
