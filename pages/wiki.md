---
layout: page
title: Wiki
description: 你还是太年轻了
keywords: 维基, Wiki
comments: false
menu: 维基
permalink: /wiki/
---

> need cheat sheet？

<ul class="listing">
{% for wiki in site.wiki %}
{% if wiki.title != "Wiki Template" %}
<li class="listing-item"><a href="{{ wiki.url }}">{{ wiki.title }}</a></li>
{% endif %}
{% endfor %}
</ul>
