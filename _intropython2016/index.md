---
collection: intropython2016
layout: default
title: intropython2016 index
---
Welcome!
{% for page in site.intropython2016 limit:3 %}
      <li>
        <a href="{{ page.url }}">{{ album.title }}</a>
        <p>{{ album.short-description }}</p>
      </li>
{% endfor %}
