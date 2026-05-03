---
permalink: /
title: ""
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

> **和水一样，虽然无味，愿耐长久。**

---

## 最新文章

{% assign sorted_posts = site.posts | sort: "date" | reverse %}
{% for post in sorted_posts limit:3 %}
- [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%Y-%m-%d" }}
{% endfor %}
{% if site.posts.size == 0 %}
*暂无文章*
{% endif %}

[查看全部 →](/year-archive/)

---

## 最新作品

{% assign sorted_portfolio = site.portfolio | sort: "date" | reverse %}
{% for item in sorted_portfolio limit:3 %}
- [{{ item.title }}]({{ item.url }}) - {{ item.date | date: "%Y-%m-%d" }}
{% endfor %}
{% if site.portfolio.size == 0 %}
*暂无作品*
{% endif %}

[查看全部 →](/portfolio/)
