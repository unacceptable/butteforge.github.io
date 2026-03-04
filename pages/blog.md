---
layout: default
title: Blog
permalink: /blog.html
---

# Blog

{% if site.posts.size > 0 %}
<div class="blog-list">
{% for post in site.posts %}
<div class="blog-card">
<h3 class="blog-card-title"><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
<p class="blog-card-meta">{{ post.date | date: "%B %-d, %Y" }}{% if post.author.name %} &middot; {{ post.author.name }}{% endif %}</p>
<p class="blog-card-excerpt">{{ post.excerpt | strip_html | truncatewords: 40 }}</p>
<a href="{{ post.url | relative_url }}" class="blog-card-link">Read more</a>
</div>
{% endfor %}
</div>
{% else %}
No posts yet. Check back soon.
{% endif %}
