---
title: nullstars devlog
---

# the devlog

here lies the dumping ground for the nullstars devlog. 

---

{% for post in site.categories.nullstars %}
<div class="blog-item">
	<h2 class="blog-title"><a href="{{ post.url }}">{{ post.title }}</a></h2>
	<div class="blog-desc">{{ post.excerpt }}</div>
</div>
{% endfor %}
