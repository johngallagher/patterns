# Rails Patterns

{% assign pages = site.patterns | where: "categories", "Rails" %}
{% for page in pages %}
- [{{ page.name }}]({{ page.url }})
{% endfor %}
