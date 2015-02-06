---
layout: page
title: Reified reasonings on software development
tagline: by Robert Taylor
---

{% for post in site.posts %}
  <article class="post">    
    
    <h1><a href="{{ post.url }}">{{ post.title }}</a></h1>

    <div class="entry">
      {{ post.content | truncatewords:75}}
    </div>
    
    <a href="{{ post.url }}" class="read-more">Read More</a>
  </article>
{% endfor %}

