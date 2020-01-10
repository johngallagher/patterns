# RSpec Patterns

{% assign pages = site.patterns | where: "categories", "RSpec" %}
{% for page in pages %}
- [{{ page.name }}]({{ page.url }})
{% endfor %}
