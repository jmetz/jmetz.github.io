---
layout: default
---

# All posts

{% for post in site.posts %}
* ({{ post.title }})["{{post.url}}"]
</ul>
{% endfor %}
