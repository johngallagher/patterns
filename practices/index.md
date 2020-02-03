---
layout: category
title: Our Practices
sidebar_sort_order: 70
---

{% assign pages = site.patterns | where: "categories", "Practices" %}
{% for page in pages %}
- [{{ page.name }}]({{ page.url | relative_url }})
{% endfor %}
