---
collection: intropython2016
layout: default
title: intropython2016 index
---
Welcome!

{% for page in site.intropython2016 limit:3 %}
<li>
    <a href="{{ page.url }}">{{ page.title }}</a>
    <p>{{ page.short-description }}</p>
</li>
{% endfor %}
