---
layout: page
title: About
description: Accelerator
keywords: Seven
comments: true
menu: 关于
permalink: /about/
---

我是Accelerator,

无法改变世界,却可以改变自己.

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
