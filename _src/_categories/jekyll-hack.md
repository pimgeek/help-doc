---
layout: category
---

<div>
{% assign cat-title-array=page.url | split:'/' %}
{% assign cat-title=cat-title-array[2] | replace:'-',' ' %}
{% assign cat-obj=site.categories[cat-title] %}

{% include category-links.html which-cat=cat-obj %}
</div>
