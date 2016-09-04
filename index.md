---
layout: page
title: Longnight Blog
tagline: Supporting tagline
---
{% include JB/setup %}

<div class="posts">
  {% for post in site.posts %}

    <h3><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h3>

    <span>{{ post.date| date: "%Y-%m-%d %H:%M" }}</span>

    {{ post.content }}

    <hr>


  {% endfor %}
</div>
