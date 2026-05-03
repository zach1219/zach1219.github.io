---
permalink: /
title: ""
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

> **[占位] 请在这里写你的自我介绍**

---

## 最新文章

{% assign sorted_posts = site.posts | sort: "date" | reverse %}
{% for post in sorted_posts limit:3 %}
- **{{ post.date | date: "%Y-%m-%d" }}** — [{{ post.title }}]({{ post.url }})
{% endfor %}
{% if site.posts.size == 0 %}
*暂无文章*
{% endif %}

[查看全部 →](/year-archive/)

---

## 最新作品

{% assign sorted_portfolio = site.portfolio | sort: "date" | reverse %}
{% for item in sorted_portfolio limit:3 %}
- **{{ item.date | date: "%Y-%m-%d" }}** — [{{ item.title }}]({{ item.url }})
{% endfor %}
{% if site.portfolio.size == 0 %}
*暂无作品*
{% endif %}

[查看全部 →](/portfolio/)
