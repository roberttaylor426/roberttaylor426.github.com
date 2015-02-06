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
	<div class="aligncenter" style="width:600px;height:0;border-top:2px dashed #BBB;font-size:0;margin-top:20px;margin-bottom:25px">-</div>

  </article>
{% endfor %}

