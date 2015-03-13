---
layout: default
---
{% include JB/setup %}

<!-- show latest post -->
{% for post in site.posts limit:1 %}
  {% assign page = post %}
  {% assign content = post.content %}
  {% include themes/sidebar/post.html %}
{% endfor %}
