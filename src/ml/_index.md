---
title: 'Machine Learning'
layout: 'collection'
tags: 'section'
---
<ul>
{%- for post in collections.ml -%}
  <li><a href="{{ post.url }}">{{ post.data.title }}</a></li>
{%- endfor -%}
</ul>