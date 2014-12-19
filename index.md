---
layout: page
title: 小新学技术
tagline: 千教万教教人求真，千学万学学做真人~~
---
{% include JB/setup %}
    
## 日志

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


