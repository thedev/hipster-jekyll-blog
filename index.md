---
title: thedev.ro by Alexandru Ionica
layout: default
---
{% for post in site.posts %}
<article> 
  <h2>{{ post.title }} </h2>
  <h4>{{ post.date | date_to_long_string }}</h4>
 </article>
{% endfor %}
