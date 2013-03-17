---
layout: page
title: scriptcs
tagline: Official team blog
---
{% include JB/setup %}

{% for post in site.posts limit 4 %}
<h1><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h1>
{{ post.content }}
{% endfor %}
