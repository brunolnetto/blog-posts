<!-- docs/index.md -->
---
layout: default
title: Github Blog
---

# Github Blog

{% for post in site.posts %}
- [{{ post.title }}]({{ post.url }}) — {{ post.date | date: "%B %d, %Y" }}
{% endfor %}
