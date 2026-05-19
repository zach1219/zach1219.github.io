---
permalink: /
title: ""
author_profile: false
redirect_from: 
  - /about/
  - /about.html
---

> **鍜屾按涓€鏍凤紝铏界劧鏃犲懗锛屾効鑰愰暱涔呫€?*

---

## 鏈€鏂版枃绔?

{% assign sorted_posts = site.posts | sort: "date" | reverse %}
{% for post in sorted_posts limit:3 %}
- [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%Y-%m-%d" }}
{% endfor %}
{% if site.posts.size == 0 %}
*鏆傛棤鏂囩珷*
{% endif %}

[鏌ョ湅鍏ㄩ儴 鈫抅(/year-archive/)

---

## 鏈€鏂颁綔鍝?

{% assign sorted_portfolio = site.portfolio | sort: "date" | reverse %}
{% for item in sorted_portfolio limit:3 %}
- [{{ item.title }}]({{ item.url }}) - {{ item.date | date: "%Y-%m-%d" }}
{% endfor %}
{% if site.portfolio.size == 0 %}
*鏆傛棤浣滃搧*
{% endif %}

[鏌ョ湅鍏ㄩ儴 鈫抅(/portfolio/)
