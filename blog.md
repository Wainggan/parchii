---
title: dumpster
styles: /asset/blog.css
---

# the dumpster

free incessant ramblings

---

{% for post in site.posts %}
<div class="blog-item">
	<h2 class="blog-title"><a href="{{ post.url }}">{{ post.title }}</a></h2>
	<div class="blog-desc">{{ post.excerpt }}</div>
</div>
{% endfor %}
