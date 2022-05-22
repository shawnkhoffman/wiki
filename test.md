---
title: Test
authors: Shawn Hoffman
editors: 
allow_edits: false
summary: 
tags: [About]
keywords: About
references: 
permalink: /test
toc: true

sidebar: home_sidebar
---

{% assign all = site.pages | sort: 'updated' | reverse | limit: 5 %}
<ul>
  {% for page in all %}
    <li>{{ page.updated }} - {{ page.title }}</li>
  {% endfor %}
</ul>
