---
layout: page
title: 
tagline: 认识自己，改变自己，接受自己，爱自己
---
{% include JB/setup %}



文章列表

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>



