---
layout: default
title: Archive
---

# Archive

{% assign postsByYear = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
{% for year in postsByYear %}
## {{ year.name }}
<ul class="post-list">
{% for post in year.items %}
<li>
  <a href="{{ post.url }}">{{ post.title }}</a>
  <span class="date">{{ post.date | date: "%d %b %Y" }}</span>
</li>
{% endfor %}
</ul>
{% endfor %}