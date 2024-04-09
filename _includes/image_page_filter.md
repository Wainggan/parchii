
{% assign items = site.data.image
	| sort: 'created'
%}
{% if include.filter %}
{% assign items = site.data.image
	| where_exp: 'item', 'item.tags contains include.tag'
%}
{% endif %}


{% for item in items reversed %}

---

{% include picture.html src=item.url alt=item.alt %}

{{ item.note }}

| created | {{ item.created | date_to_string }} |
| tags | {% for tag in item.tags -%}
	{%- if site.data.image_tags contains tag -%}
	[{{ tag }}](./{{tag}}.html)
	{%- else -%}
	{{ tag }}
	{%- endif -%}
	{% unless forloop.last %}, {% endunless -%}
{% endfor %} |

| | tools |
{% for type in item.tools -%}
| {{ type[0] }} | {{type[1]}} |
{% endfor %}


{% endfor %}
