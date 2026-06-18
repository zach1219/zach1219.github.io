---
permalink: /
title: ""
author_profile: false
redirect_from: 
  - /about/
  - /about.html
---

<!-- Apple-style Navigation -->
<nav class="home-nav">
  <a class="nav-logo" href="/">好好吃饭</a>
  <div class="nav-links">
    <a href="/year-archive/">文章</a>
    <a href="/portfolio/">作品</a>
    <a href="https://github.com/zach1219" target="_blank">GitHub</a>
  </div>
</nav>

<!-- Hero Section -->
<section class="apple-hero">
  <div class="apple-hero-content">
    <h1>好好吃饭</h1>
    <p class="hero-subtitle">和水一样，虽然无味，愿耐长久。</p>
    <a href="#posts" class="hero-cta">探索文章</a>
  </div>
  <div class="apple-scroll-indicator">
    <svg viewBox="0 0 24 24"><polyline points="6 9 12 15 18 9"></polyline></svg>
  </div>
</section>

<!-- Posts Section -->
<section class="apple-content" id="posts">
  <div class="apple-container">
    <div class="apple-section-header apple-reveal">
      <h2>最新文章</h2>
      <p>记录生活与思考</p>
    </div>

    <div class="apple-posts-grid">
      {% assign sorted_posts = site.posts | sort: "date" | reverse %}
      {% for post in sorted_posts limit:6 %}
      <a href="{{ post.url }}" class="apple-post-card apple-reveal">
        <div class="card-date">{{ post.date | date: "%Y-%m-%d" }}</div>
        <div class="card-title">{{ post.title }}</div>
        {% if post.excerpt %}
        <div class="card-excerpt">{{ post.excerpt | strip_html | truncatewords: 30 }}</div>
        {% endif %}
        {% if post.tags %}
        <div class="card-tags">
          {% for tag in post.tags limit:3 %}
          <span class="card-tag">{{ tag }}</span>
          {% endfor %}
        </div>
        {% endif %}
      </a>
      {% endfor %}
    </div>

    {% if site.posts.size > 6 %}
    <div class="apple-view-all apple-reveal">
      <a href="/year-archive/">查看全部文章</a>
    </div>
    {% endif %}
  </div>
</section>

<!-- Quote Section -->
<section class="apple-quote-section">
  <div class="apple-quote apple-reveal">
    <blockquote>生活慢下来，才懂滋味。</blockquote>
    <p class="quote-author">好好吃饭</p>
  </div>
</section>

<!-- Scroll Reveal Script -->
<script>
document.addEventListener('DOMContentLoaded', function() {
  var reveals = document.querySelectorAll('.apple-reveal');
  var observer = new IntersectionObserver(function(entries) {
    entries.forEach(function(entry) {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
      }
    });
  }, { threshold: 0.15 });
  reveals.forEach(function(el) { observer.observe(el); });
});
</script>
