---
layout: category
title: Rails
sidebar_sort_order: 20
---

{% assign pages = site.patterns | where: "categories", "Rails" %}
{% for page in pages %}
- [{{ page.name }}]({{ page.url | relative_url }})
{% endfor %}
