---
layout: posts
title: Posts
---

{% for post in site.posts %}

<h2>{{ post.title }}</h2>

<p>{{ post.date }}</p>

<p>{{ post.content }}</p>

{% endfor %}
