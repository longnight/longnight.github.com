---
layout: page
title: Longnight Blog
tagline: Supporting tagline
---
{% include JB/setup %}

<div class="posts">
  {% for post in site.posts %}

    <h4><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h4>
    <span>{{ post.date| date: "%Y-%m-%d %H:%M" }}</span>
    <br>
    <br>

    {{ post.content }}

    <hr>
    <hr>
    <br>
    <br>


  {% endfor %}
</div>
