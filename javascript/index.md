# Javascript Patterns

{% assign pages = site.patterns | where: "categories", "Javascript" %}
{% for page in pages %}
- [{{ page.name }}]({{ page.url }})
{% endfor %}
