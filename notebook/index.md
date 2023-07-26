---
layout: page
title: Notebook
description: Elyanah Aco's Blog - Notebook Landing Page
permalink: /notebook/
---

This is where I document my experiments and write my notes and thoughts 
on topics that interest me.

I'll try to keep each post either data- or coding-centric. Check out my Life
page for other kinds of musings.
<ul>
  {% for post in site.categories.notebook %}
    <li>
        <span>{{ post.date | date_to_string }}</span> » {% if post.highlight %}&starf; {% endif %}<a href="{{ post.url }}" title="{{ post.title }}">{{ post.title | truncate:72 }}</a>
    </li>
  {% endfor %}
</ul>
