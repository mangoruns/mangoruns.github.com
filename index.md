---
layout: page
title: ❤碎碎念
tagline: too hot!!!
---
{% include JB/setup %}

> ![](http://i.imgur.com/yvcYahj.jpg)
>  
> 发生的都值得回味，花开值的结果。
>  
> 我但愿人生常能回味，而我不回头。
>                                 
>  —— 张悬



<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>



