# Ruby Patterns

{% assign pages = site.patterns | where: "categories", "Ruby" %}
{% for page in pages %}
- [{{ page.name }}]({{ page.url }})
{% endfor %}
