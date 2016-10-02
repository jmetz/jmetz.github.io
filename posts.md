---
---
{% for post in site.posts %}
<ul>
* ({{ post.title }})["{{post.url}}"]
</ul>
{% endfor %}
