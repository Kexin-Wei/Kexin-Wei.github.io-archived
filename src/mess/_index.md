---
title: 'What a mess: every tiny thing is here'
layout: 'collection'
tags: 'section'
---
<ul>
{%- for post in collections.mess -%}
  <li><a href="{{ post.url }}">{{ post.data.title }}</a></li>
{%- endfor -%}
</ul>