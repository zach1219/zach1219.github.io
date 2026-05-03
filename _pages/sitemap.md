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
}
.sitemap-hero h1 {
  font-size: 2.8rem;
  font-weight: 700;
  letter-spacing: -0.03em;
  color: #1d1d1f;
  margin-bottom: 0.5rem;
}
.sitemap-hero p {
  font-size: 1.1rem;
  color: #86868b;
}
.mindmap {
  max-width: 800px;
  margin: 2rem auto;
  padding: 0 1rem;
}
.mindmap-root {
  text-align: center;
  padding: 1.5rem 2rem;
  background: linear-gradient(135deg, #0066cc, #0077ed);
  color: #fff;
  border-radius: 16px;
  font-size: 1.3rem;
  font-weight: 600;
  margin-bottom: 2rem;
  box-shadow: 0 4px 16px rgba(0, 102, 204, 0.2);
}
.mindmap-branch {
  margin-bottom: 2.5rem;
  padding-left: 1.5rem;
  border-left: 3px solid #e5e5e7;
}
.mindmap-branch-title {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 1.2rem;
  font-weight: 600;
  color: #1d1d1f;
  margin-bottom: 1rem;
  padding: 0.4rem 1rem;
  background: #f5f5f7;
  border-radius: 8px;
  position: relative;
  left: -1.5rem;
}
.mindmap-branch-title i {
  color: #0066cc;
}
.mindmap-node {
  display: flex;
  align-items: center;
  padding: 0.6rem 0;
  margin-left: 0.5rem;
  position: relative;
}
.mindmap-node::before {
  content: '';
  width: 8px;
  height: 8px;
  background: #0066cc;
  border-radius: 50%;
  margin-right: 1rem;
  flex-shrink: 0;
  opacity: 0.6;
}
.mindmap-node a {
  color: #1d1d1f;
  font-size: 1rem;
  text-decoration: none;
  transition: color 0.2s;
}
.mindmap-node a:hover {
  color: #0066cc;
}
.mindmap-node .date {
  margin-left: auto;
  color: #86868b;
  font-size: 0.8rem;
  padding-left: 1rem;
  white-space: nowrap;
}
.mindmap-empty {
  color: #86868b;
  font-style: italic;
  margin-left: 2rem;
  padding: 0.5rem 0;
}
.mindmap-footer {
  text-align: center;
  margin-top: 3rem;
  padding-top: 2rem;
  border-top: 1px solid #e5e5e7;
}
.mindmap-footer a {
  color: #0066cc;
  font-size: 0.9rem;
}
@media (max-width: 768px) {
  .mindmap-branch { padding-left: 1rem; }
  .mindmap-branch-title { left: -1rem; font-size: 1rem; }
  .mindmap-node .date { display: none; }
}
</style>

<div class="sitemap-hero">
  <h1>{{ site.title }}</h1>
  <p>站点所有内容索引</p>
</div>

<div class="mindmap">
  <div class="mindmap-root">{{ site.title }}</div>

  <!-- Posts -->
  <div class="mindmap-branch">
    <div class="mindmap-branch-title">✏️ 文章</div>
    {% if site.posts.size > 0 %}
      {% for post in site.posts %}
        <div class="mindmap-node">
          <a href="{{ base_path }}{{ post.url }}">{{ post.title }}</a>
          <span class="date">{{ post.date | date: "%Y-%m-%d" }}</span>
        </div>
      {% endfor %}
    {% else %}
      <p class="mindmap-empty">暂无文章</p>
    {% endif %}
  </div>

  <!-- Collections -->
  {% for collection in site.collections %}
    {% unless collection.output == false or collection.label == "posts" %}
      <div class="mindmap-branch">
        <div class="mindmap-branch-title">📂 {{ collection.label }}</div>
        {% for post in collection.docs %}
          <div class="mindmap-node">
            <a href="{{ base_path }}{{ post.url }}">{{ post.title }}</a>
            {% if post.date %}<span class="date">{{ post.date | date: "%Y-%m-%d" }}</span>{% endif %}
          </div>
        {% endfor %}
      </div>
    {% endunless %}
  {% endfor %}
</div>

<div class="mindmap-footer">
  <a href="{{ base_path }}/sitemap.xml">📡 XML 站点地图</a>
</div>
