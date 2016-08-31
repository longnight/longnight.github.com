---
layout: page
title: Longnight Blog
tagline: Supporting tagline
---
{% include JB/setup %}

<div class="posts">
  {% for post in site.posts %}
    <p><span>{{ post.date }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></p>
  {% endfor %}
</div>
