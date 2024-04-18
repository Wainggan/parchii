---
title: "ardency wiki: world"
---

# world

important places in the world.


## index

{% assign folder = site.pages | where_exp: "page", "page.url contains '/ardency/wiki/world' " %}
{% for page in folder %}{% if page.name != "index.md" %}
- [{{ page.name }}]({{ page.url }})
{%- endif -%}
{%- endfor %}

