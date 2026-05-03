---
layout: default
permalink: /search/
title: ""
---

<style>
.search-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.4);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  z-index: 200;
  display: flex;
  justify-content: center;
  padding-top: 15vh;
  animation: fadeIn 0.2s ease;
}
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}
@keyframes slideDown {
  from { opacity: 0; transform: translateY(-20px); }
  to { opacity: 1; transform: translateY(0); }
}
.search-container {
  width: 100%;
  max-width: 680px;
  padding: 0 1rem;
  animation: slideDown 0.25s ease;
}
.search-box {
  background: #fff;
  border-radius: 14px;
  box-shadow: 0 8px 40px rgba(0, 0, 0, 0.12);
  overflow: hidden;
}
.search-input-wrap {
  display: flex;
  align-items: center;
  padding: 0 1.2rem;
  border-bottom: 1px solid #e5e5e7;
}
.search-input-wrap i {
  color: #86868b;
  font-size: 1.1rem;
  margin-right: 0.8rem;
}
.search-input {
  flex: 1;
  border: none;
  outline: none;
  font-size: 1.1rem;
  padding: 1rem 0;
  font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", sans-serif;
  color: #1d1d1f;
  background: transparent;
}
.search-input::placeholder {
  color: #86868b;
}
.search-close {
  background: none;
  border: none;
  color: #86868b;
  font-size: 0.85rem;
  cursor: pointer;
  padding: 0.3rem 0.6rem;
  border-radius: 6px;
  transition: background 0.2s;
}
.search-close:hover {
  background: #f5f5f7;
}
.search-results {
  max-height: 50vh;
  overflow-y: auto;
  padding: 0.5rem 0;
}
.search-result {
  display: block;
  padding: 0.8rem 1.2rem;
  text-decoration: none;
  transition: background 0.15s;
  border-bottom: 1px solid #f5f5f7;
}
.search-result:last-child {
  border-bottom: none;
}
.search-result:hover {
  background: #f5f5f7;
}
.search-result-title {
  font-size: 1rem;
  font-weight: 500;
  color: #1d1d1f;
  margin-bottom: 0.2rem;
}
.search-result-meta {
  font-size: 0.8rem;
  color: #86868b;
  display: flex;
  gap: 0.8rem;
}
.search-result-excerpt {
  font-size: 0.85rem;
  color: #515154;
  margin-top: 0.3rem;
  line-height: 1.5;
}
.search-empty {
  text-align: center;
  padding: 2rem 1rem;
  color: #86868b;
}
.search-empty i {
  font-size: 2rem;
  margin-bottom: 0.5rem;
  display: block;
}
.search-hint {
  padding: 1rem 1.2rem;
  color: #86868b;
  font-size: 0.85rem;
  text-align: center;
}
.search-kbd {
  display: inline-block;
  background: #f5f5f7;
  border: 1px solid #d2d2d7;
  border-radius: 5px;
  padding: 0.1rem 0.4rem;
  font-size: 0.75rem;
  font-family: monospace;
  color: #515154;
  margin: 0 0.1rem;
}
</style>

<div class="search-overlay" id="searchOverlay" onclick="if(event.target===this)closeSearch()">
  <div class="search-container">
    <div class="search-box">
      <div class="search-input-wrap">
        <i class="fa fa-search"></i>
        <input type="text" class="search-input" id="searchInput" placeholder="搜索文章和页面..." autofocus oninput="doSearch(this.value)">
        <button class="search-close" onclick="closeSearch()">
          <span class="search-kbd">ESC</span>
        </button>
      </div>
      <div class="search-results" id="searchResults">
        <div class="search-hint">输入关键词开始搜索</div>
      </div>
    </div>
  </div>
</div>

<script>
let searchData = null;

// Load search index
fetch('/search.json')
  .then(r => r.json())
  .then(data => { searchData = data; })
  .catch(() => {});

function doSearch(query) {
  const results = document.getElementById('searchResults');
  if (!query.trim()) {
    results.innerHTML = '<div class="search-hint">输入关键词开始搜索</div>';
    return;
  }
  if (!searchData) {
    results.innerHTML = '<div class="search-empty"><i class="fa fa-spinner fa-spin"></i>加载中...</div>';
    return;
  }
  const q = query.toLowerCase();
  const matches = searchData.filter(item =>
    item.title.toLowerCase().includes(q) ||
    item.excerpt.toLowerCase().includes(q)
  );
  if (matches.length === 0) {
    results.innerHTML = '<div class="search-empty"><i class="fa fa-search"></i>没有找到相关内容</div>';
    return;
  }
  results.innerHTML = matches.map(item => `
    <a class="search-result" href="${item.url}">
      <div class="search-result-title">${item.title}</div>
      <div class="search-result-meta">
        <span>${item.type}</span>
        ${item.date ? '<span>' + item.date + '</span>' : ''}
      </div>
      ${item.excerpt ? '<div class="search-result-excerpt">' + item.excerpt.substring(0, 100) + '...</div>' : ''}
    </a>
  `).join('');
}

function closeSearch() {
  window.history.back();
}

document.addEventListener('keydown', e => {
  if (e.key === 'Escape') closeSearch();
});
</script>
