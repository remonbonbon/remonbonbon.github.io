---
layout: default
---
{% include JB/setup %}
<link rel="stylesheet" href="{{ BASE_PATH }}/assets/index.css" media="all">

<!-- show recent posts -->
{% for post in site.posts limit:10 %}
<article class="abstract_box">
  <header class="title">
    <h3><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }} {% if post.tagline %}<small>{{post.tagline}}</small>{% endif %}</a></h3>
  </header>
  <div class="detail">
    <div>{{ post.description }}</div>
    <span class="date">Published {{ post.date | date: "%Y-%m-%d" }}</span>
  {% unless post.categories == empty %}
    <ul class="tag_box inline">
      <li><span class="glyphicon glyphicon-folder-open" aria-hidden="true"></span> </li>
      {% assign categories_list = post.categories %}
      {% include JB/categories_list %}
    </ul>
  {% endunless %}
  {% unless post.tags == empty %}
    <ul class="tag_box inline">
      <li><span class="glyphicon glyphicon-tags" aria-hidden="true"></span> </li>
      {% assign tags_list = post.tags %}
      {% include JB/tags_list %}
    </ul>
  {% endunless %}
  </div>
</article>
{% endfor %}

<div>
<a href="{{ BASE_PATH }}{{ site.JB.archive_path }}">and more posts</a>
</div>
