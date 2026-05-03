---
permalink: /
title: "首页"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

# 👋 关于我

> **[待补充] 请在此处填写你的自我介绍**
> 
> 写好后告诉我，我帮你更新进来。

---

## 📝 最新博文

{% assign sorted_posts = site.posts | sort: "date" | reverse %}
{% for post in sorted_posts limit:3 %}
- **{{ post.date | date: "%Y-%m-%d" }}** — [{{ post.title }}]({{ post.url }})
{% endfor %}
{% if site.posts.size == 0 %}
*暂无博文*
{% endif %}

[查看全部博文 →](/year-archive/)

---

## 📚 最新出版物

{% assign sorted_pubs = site.publications | sort: "date" | reverse %}
{% for pub in sorted_pubs limit:3 %}
- **{{ pub.date | date: "%Y-%m-%d" }}** — [{{ pub.title }}]({{ pub.url }})
{% endfor %}
{% if site.publications.size == 0 %}
*暂无出版物*
{% endif %}

[查看全部出版物 →](/publications/)

---

## 🎓 最新教程

{% assign sorted_teaching = site.teaching | sort: "date" | reverse %}
{% for item in sorted_teaching limit:3 %}
- **{{ item.date | date: "%Y-%m-%d" }}** — [{{ item.title }}]({{ item.url }})
{% endfor %}
{% if site.teaching.size == 0 %}
*暂无教程*
{% endif %}

[查看全部教程 →](/teaching/)

---

## 💬 最新留言

{% assign sorted_talks = site.talks | sort: "date" | reverse %}
{% for talk in sorted_talks limit:3 %}
- **{{ talk.date | date: "%Y-%m-%d" }}** — [{{ talk.title }}]({{ talk.url }})
{% endfor %}
{% if site.talks.size == 0 %}
*暂无留言*
{% endif %}

[查看全部留言 →](/talks/)

---

## 📁 最新文件夹

{% assign sorted_portfolio = site.portfolio | sort: "date" | reverse %}
{% for item in sorted_portfolio limit:3 %}
- **{{ item.date | date: "%Y-%m-%d" }}** — [{{ item.title }}]({{ item.url }})
{% endfor %}
{% if site.portfolio.size == 0 %}
*暂无文件夹内容*
{% endif %}

[查看全部文件夹 →](/portfolio/)
