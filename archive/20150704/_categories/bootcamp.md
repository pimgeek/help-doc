---
layout: category
---

{% assign cat-title-array=page.url | split:'/' %}
{% assign cat-title=cat-title-array[2] | replace:'-',' ' %}
{% assign cat-obj=site.categories[cat-title] %}
{% include category-articles.html which-cat=cat-obj %}
