---
layout: page
title: "Tags"
description: "哈哈，你找到了我的文章基因库"  
header-img: "img/semantic.jpg"  
---

##本页使用方法

1. 在下面选一个你喜欢的词
2. 点击它
3. 相关的文章会「唰」地一声跳到页面顶端
4. 马上试试？

<div id="post-list">

{% for tag in site.tags %}
<h2 id="{{ tag[0] }}">{{ tag[0] | capitalize }}</h2>
<ul class="post-list">
{% assign pages_list = tag[1] %}

</div>
