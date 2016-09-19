---
collection: intropython2016
layout: tutorial
title: Index
---
Welcome!

This tutorial contains the following pages: 
{% for page in site.intropython2016 | shift %}
<li>
    <a href="{{ page.url }}">{{ page.title }}</a>
    <p>{{ page.short-description }}</p>
</li>
{% endfor %}
