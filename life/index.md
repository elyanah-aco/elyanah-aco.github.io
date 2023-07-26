---
layout: page
title: Life
description: Elyanah Aco's Blog - Life
permalink: /life/
---

This is where I reflect on my professional and personal life, on how I wrangle around my being
a programmer, an artist, a woman, and a Filipino. The person is not just one or another, but
an intermingling of selves.

<ul>
  {% for post in site.categories.life %}
  {% if post.type == "post" %}
    <li>
        <span>{{ post.date | date_to_string }}</span> » <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
        <meta name="description" content="{{ post.summary | escape }}">
        <meta name="keywords" content="{{ post.tags | join: ', ' | escape }}"/>
    </li>
  {% endif %}
  {% endfor %}
</ul>
