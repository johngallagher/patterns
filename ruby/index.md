---
layout: category
title: Ruby
sidebar_sort_order: 10
---

{% assign pages = site.patterns | where: "categories", "Ruby" %}
{% for page in pages %}
- [{{ page.name }}]({{ page.url | relative_url }})
{% endfor %}
