---
title: "Recording ..."
layout: 'collection'
tags: 'section'
---
<ul>
{%- for post in collections.record -%}
  <li><a href="{{ post.url }}">{{ post.data.title }}</a></li>
{%- endfor -%}
</ul>