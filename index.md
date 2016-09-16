---
layout: default
permanlink: index.html
title: Saurabh Bajaj
---

Welcome to my blog - I write about software engineering, distributed systems and data infrastructure.

### Recent blog posts

{% for post in site.posts %}    
(*{{ post.date | date: "%B %e, %Y" }}*)

### [**{{ post.title }}**]({{ post.url }})
{% endfor %}
