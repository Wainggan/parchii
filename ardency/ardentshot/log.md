---
title: ardent shot devlog
---

# devlog

the devlog for ardent shot.

---

{% for post in site.categories.ardency %}
<div class="blog-item">
	<h2 class="blog-title"><a href="{{ post.url }}">{{ post.title }}</a></h2>
	<div class="blog-desc">{{ post.excerpt }}</div>
</div>
{% endfor %}

