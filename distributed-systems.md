---
layout: page
title: "Distributed Systems"
permalink: /distributed-systems/
---

---
### Notes and Articles

{% assign posts = site.projects_en | sort: "order" %}
{% for post in posts %}
  {% if post.categories contains "distributed-systems" or post.category == "distributed-systems" or post.categories contains "sistemas-distribuidos" %}
* [{{ post.title }}]({{ post.url | relative_url }})
  {% endif %}
{% endfor %}

---
