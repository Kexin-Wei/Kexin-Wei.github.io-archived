---
title: 无极
layout: base.njk
---

A learner recording daily steps and struggles.

The records are splitted into:


<ul>
{%- for post in collections.section -%}
  <li><a href="{{ post.url }}">{{ post.data.title }}</a></li>
{%- endfor -%}
</ul>
