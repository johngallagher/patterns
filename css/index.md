# CSS Patterns

{% assign pages = site.patterns | where: "categories", "CSS" %}
{% for page in pages %}
- [{{ page.name }}]({{ page.url | relative_url }})
{% endfor %}
