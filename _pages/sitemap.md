---
layout: archive
title: "站点地图"
permalink: /sitemap/
author_profile: true
---

{% include base_path %}

<style>
.sitemap-hero {
  text-align: center;
  padding: 3rem 1rem 2rem;
  margin-bottom: 2rem;
}
.sitemap-hero h1 {
  font-size: 2.5rem;
  font-weight: 700;
  letter-spacing: -0.02em;
  color: #1d1d1f;
  margin-bottom: 0.5rem;
}
.sitemap-hero p {
  font-size: 1.1rem;
  color: #86868b;
  max-width: 500px;
  margin: 0 auto;
  line-height: 1.6;
}
.sitemap-section {
  margin-bottom: 3rem;
}
.sitemap-section h2 {
  font-size: 1.6rem;
  font-weight: 600;
  color: #1d1d1f;
  padding-bottom: 0.75rem;
  border-bottom: 1px solid #e5e5e7;
  margin-bottom: 1.2rem;
  letter-spacing: -0.01em;
}
.sitemap-section h2 i {
  margin-right: 0.5rem;
  color: #0071e3;
  font-size: 1.3rem;
}
.sitemap-list {
  list-style: none;
  padding: 0;
  margin: 0;
}
.sitemap-list li {
  border-bottom: 1px solid #f5f5f7;
}
.sitemap-list li:last-child {
  border-bottom: none;
}
.sitemap-list a {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 0.5rem;
  color: #1d1d1f;
  text-decoration: none;
  font-size: 1.05rem;
  transition: all 0.2s ease;
  border-radius: 8px;
}
.sitemap-list a:hover {
  background: #f5f5f7;
  padding-left: 1rem;
  color: #0071e3;
}
.sitemap-list .date {
  font-size: 0.85rem;
  color: #86868b;
  font-variant-numeric: tabular-nums;
  white-space: nowrap;
}
.sitemap-list .title {
  flex: 1;
  font-weight: 400;
}
.sitemap-empty {
  text-align: center;
  padding: 2rem;
  color: #86868b;
  font-style: italic;
}
.sitemap-xml {
  text-align: center;
  margin-top: 3rem;
  padding-top: 2rem;
  border-top: 1px solid #e5e5e7;
}
.sitemap-xml a {
  color: #0071e3;
  text-decoration: none;
  font-size: 0.95rem;
}
.sitemap-xml a:hover {
  text-decoration: underline;
}
</style>

<div class="sitemap-hero">
  <h1>站点地图</h1>
  <p>本站所有页面和文章的索引</p>
</div>

<div class="sitemap-section">
  <h2><i class="fa fa-file-alt"></i> 页面</h2>
  {% if site.pages.size > 0 %}
  <ul class="sitemap-list">
    {% for page in site.pages %}
      {% unless page.title == nil or page.title == "" %}
      <li>
        <a href="{{ base_path }}{{ page.url }}">
          <span class="title">{{ page.title }}</span>
        </a>
      </li>
      {% endunless %}
    {% endfor %}
  </ul>
  {% else %}
  <p class="sitemap-empty">暂无页面</p>
  {% endif %}
</div>

<div class="sitemap-section">
  <h2><i class="fa fa-pencil-alt"></i> 文章</h2>
  {% if site.posts.size > 0 %}
  <ul class="sitemap-list">
    {% for post in site.posts %}
    <li>
      <a href="{{ base_path }}{{ post.url }}">
        <span class="title">{{ post.title }}</span>
        <span class="date">{{ post.date | date: "%Y-%m-%d" }}</span>
      </a>
    </li>
    {% endfor %}
  </ul>
  {% else %}
  <p class="sitemap-empty">暂无文章</p>
  {% endif %}
</div>

{% capture written_label %}'None'{% endcapture %}
{% for collection in site.collections %}
  {% unless collection.output == false or collection.label == "posts" %}
    {% capture label %}{{ collection.label }}{% endcapture %}
    {% if label != written_label %}
      <div class="sitemap-section">
        <h2><i class="fa fa-folder-open"></i> {{ label }}</h2>
        <ul class="sitemap-list">
        {% capture written_label %}{{ label }}{% endcapture %}
    {% endif %}
    {% for post in collection.docs %}
      <li>
        <a href="{{ base_path }}{{ post.url }}">
          <span class="title">{{ post.title }}</span>
          {% if post.date %}<span class="date">{{ post.date | date: "%Y-%m-%d" }}</span>{% endif %}
        </a>
      </li>
    {% endfor %}
  {% endunless %}
{% endfor %}
</ul>
</div>

<div class="sitemap-xml">
  <a href="{{ base_path }}/sitemap.xml"><i class="fa fa-rss"></i> XML 站点地图</a>
</div>
