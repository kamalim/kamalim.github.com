---
layout: page
title : Home
header : Post Archive
group: navigation
---
{% include JB/setup %}

<h2>All Pages</h2>
<ul>
{% assign posts_collate = site.posts %}
{% include JB/posts_collate %}
</ul>
