---
layout: page
title: scriptcs
tagline: Official team blog
---
{% include JB/setup %}

{% for post in site.posts limit 4 %}
<h2><small>{{ post.date | date_to_string }} </small> <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a><small> - {{ post.tagline }}</small></h2>
{{ post.content }}
{% endfor %}
