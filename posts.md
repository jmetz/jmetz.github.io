---
layout: default
title: All Blog Posts
---
{% for post in site.posts %}
* [{{ post.title }}]({{post.url}})
{% endfor %}
