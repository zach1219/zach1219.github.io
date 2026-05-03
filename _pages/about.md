---
permalink: /
title: "棣栭〉"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

# 鍏充簬鎴?
> **[寰呰ˉ鍏匽 璇峰湪姝ゅ濉啓浣犵殑鑷垜浠嬬粛**
> 
> 鍐欏ソ鍚庡憡璇夋垜锛屾垜甯綘鏇存柊杩涙潵銆?
---

## 馃摑 鏈€鏂板洖蹇嗗綍

{% assign sorted_posts = site.posts | sort: "date" | reverse %}
{% for post in sorted_posts limit:3 %}
- **{{ post.date | date: "%Y-%m-%d" }}** 鈥?[{{ post.title }}]({{ post.url }})
{% endfor %}
{% if site.posts.size == 0 %}
*鏆傛棤鍥炲繂褰?
{% endif %}

[鏌ョ湅鍏ㄩ儴鍥炲繂褰?鈫抅(/year-archive/)

---

## 馃柤锔?鏈€鏂癎allery

{% assign sorted_portfolio = site.portfolio | sort: "date" | reverse %}
{% for item in sorted_portfolio limit:3 %}
- **{{ item.date | date: "%Y-%m-%d" }}** 鈥?[{{ item.title }}]({{ item.url }})
{% endfor %}
{% if site.portfolio.size == 0 %}
*鏆傛棤Gallery鍐呭*
{% endif %}

[鏌ョ湅鍏ㄩ儴Gallery 鈫抅(/portfolio/)