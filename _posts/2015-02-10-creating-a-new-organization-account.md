---
layout: post
categories: ['Setting up and managing organizations and teams']
---

You can create a new organization by either setting up a new organization or converting an existing personal account into an organization.

{% assign coll-title-array=page.url | split:'/' %}
{% assign coll-title=coll-title-array[2] | replace:'-','_' | downcase %}
{% assign coll-obj = site.collections[coll-title].docs %}
{% include sub-articles.html which-coll=coll-obj %}
