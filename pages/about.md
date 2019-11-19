---
layout: page
title: About
description: 前端酒馆
keywords: Sen Wang, 王森
comments: true
menu: 关于
permalink: /about/
---

我是王森，前端小学习，一直在学习的路上。

DRY。

种一棵树最好的时候是十年前，其次是现在。

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
