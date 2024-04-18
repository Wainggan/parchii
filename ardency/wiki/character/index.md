---
title: "ardency wiki: characters"
---

# characters

anyone who has some noteriety, importance, or notable appearance in a story.


## index

{% assign folder = site.pages | where_exp: "page", "page.url contains '/ardency/wiki/character' " %}
{% for page in folder %}{% if page.name != "index.md" %}
- [{{ page.name }}]({{ page.url }})
{%- endif -%}
{%- endfor %}

