---
permalink: /
title: "Home"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

# About Me

> **[PLACEHOLDER] Please write your self-introduction here**
> 
> Let me know when it's ready and I'll update it for you.

---

## Latest Memoir

{% assign sorted_posts = site.posts | sort: "date" | reverse %}
{% for post in sorted_posts limit:3 %}
- **{{ post.date | date: "%Y-%m-%d" }}** — [{{ post.title }}]({{ post.url }})
{% endfor %}
{% if site.posts.size == 0 %}
*No memoir entries yet*
{% endif %}

[View all →](/year-archive/)

---

## Latest Gallery

{% assign sorted_portfolio = site.portfolio | sort: "date" | reverse %}
{% for item in sorted_portfolio limit:3 %}
- **{{ item.date | date: "%Y-%m-%d" }}** — [{{ item.title }}]({{ item.url }})
{% endfor %}
{% if site.portfolio.size == 0 %}
*No gallery items yet*
{% endif %}

[View all →](/portfolio/)