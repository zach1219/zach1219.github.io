---
layout: archive
title: "站点地图"
permalink: /sitemap/
author_profile: false
---

{% include base_path %}

<style>
.stats-bar {
  display: flex;
  gap: 2rem;
  justify-content: center;
  flex-wrap: wrap;
  padding: 1.5rem 0;
  margin-bottom: 2rem;
  border-bottom: 1px solid #e5e5e7;
}
.stat-item {
  text-align: center;
}
.stat-num {
  font-size: 1.8rem;
  font-weight: 700;
  color: #1d1d1f;
}
.stat-label {
  font-size: 0.8rem;
  color: #86868b;
  margin-top: 0.2rem;
}
.mindmap {
  max-width: 800px;
  margin: 0 auto;
  padding: 0 1rem;
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
.mindmap-node {
  display: flex;
  align-items: center;
  padding: 0.6rem 0;
  margin-left: 0.5rem;
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
}
@media (prefers-color-scheme: dark) {
  .stats-bar { border-bottom-color: #38383d; }
  .stat-num { color: #f5f5f7; }
  .stat-label { color: #86868b; }
  .mindmap-branch { border-left-color: #38383d; }
  .mindmap-branch-title { background: #2c2c2e; color: #f5f5f7; }
  .mindmap-node a { color: #f5f5f7; }
  .mindmap-node a:hover { color: #2997ff; }
  .mindmap-node::before { background: #2997ff; }
}
</style>

<div class="stats-bar">
  <div class="stat-item">
    <div class="stat-num">{{ site.posts | size }}</div>
    <div class="stat-label">文章</div>
  </div>
  <div class="stat-item">
    <div class="stat-num">{{ site.categories | size }}</div>
    <div class="stat-label">分类</div>
  </div>
  <div class="stat-item">
    <div class="stat-num">{{ site.tags | size }}</div>
    <div class="stat-label">标签</div>
  </div>
  <div class="stat-item">
    <div class="stat-num">{{ site.time | date: "%Y" }}</div>
    <div class="stat-label">年份</div>
  </div>
</div>

<div class="mindmap">
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
