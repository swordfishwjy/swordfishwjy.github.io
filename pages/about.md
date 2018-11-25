---
layout: page
title: 逗逼在此
description: 何以解忧，唯有学习
keywords: Jianyu Wang, 王剑宇
comments: true
menu: About Me
permalink: /about/
---

今天比昨天更博学了么？

## Find me anyway

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skills

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
