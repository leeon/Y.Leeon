---
layout: page
title: App
description: 分类
---

<ul class="archive">
{% for cat in site.categories %}
    {% if cat[0] == "app" %}
    	{% for post in cat[1] %}
    	<li class="item">
    		<time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
    		<a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
    	</li>
    	{% endfor %}
    {% endif %}
{% endfor %}
</ul>
