---
layout: home
title: homepage
---

<h1>全站文章列表</h1>

<ul>
{% for article in site.articles %}
  <li>
    <a href="{{ article.url | prepend: site.baseurl }}">{{ article.title }}</a>
  </li>
{% endfor %}
</ul>

<hr />
