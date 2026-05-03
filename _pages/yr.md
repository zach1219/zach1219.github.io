---
layout: null
exclude_from_search: true
permalink: /yr/
---
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>管理后台</title>
<style>
* { margin: 0; padding: 0; box-sizing: border-box; }
body {
  font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", "Helvetica Neue", "PingFang SC", "Microsoft YaHei", sans-serif;
  background: #f5f5f7;
  color: #1d1d1f;
  min-height: 100vh;
}
@media (prefers-color-scheme: dark) {
  body { background: #1d1d1f; color: #f5f5f7; }
  .login-box, .admin-card, .modal-content { background: #2c2c2e !important; border-color: #38383d !important; }
  input, textarea, select { background: #1d1d1f !important; color: #f5f5f7 !important; border-color: #48484a !important; }
  .admin-card h3 { color: #f5f5f7 !important; }
  .stat-num { color: #f5f5f7 !important; }
  .log-item { border-bottom-color: #38383d !important; color: #a1a1a6 !important; }
  .btn-logout { background: #3a3a3c !important; color: #f5f5f7 !important; }
}
.login-wrap {
  display: flex; align-items: center; justify-content: center;
  min-height: 100vh; padding: 1rem;
}
.login-box {
  background: #fff; border-radius: 16px; padding: 3rem 2.5rem;
  box-shadow: 0 8px 40px rgba(0,0,0,0.08); width: 100%; max-width: 380px;
  text-align: center;
}
.login-box h1 { font-size: 1.8rem; font-weight: 700; margin-bottom: 0.5rem; }
.login-box p { color: #86868b; margin-bottom: 2rem; font-size: 0.9rem; }
.login-box input {
  width: 100%; padding: 0.8rem 1rem; border: 1px solid #d2d2d7;
  border-radius: 10px; font-size: 1rem; margin-bottom: 1rem;
  outline: none; transition: border-color 0.2s;
  font-family: inherit;
}
.login-box input:focus { border-color: #0066cc; }
.login-box button {
  width: 100%; padding: 0.8rem; background: #0066cc; color: #fff;
  border: none; border-radius: 10px; font-size: 1rem; font-weight: 600;
  cursor: pointer; transition: background 0.2s;
}
.login-box button:hover { background: #0077ed; }
.login-error { color: #ff3b30; font-size: 0.85rem; margin-bottom: 1rem; display: none; }
.admin-wrap { display: none; max-width: 1100px; margin: 0 auto; padding: 6rem 1.5rem 2rem; }
.admin-header {
  display: flex; justify-content: space-between; align-items: center;
  margin-bottom: 2rem; padding-bottom: 1rem; border-bottom: 1px solid #e5e5e7;
}
.admin-header h1 { font-size: 1.6rem; font-weight: 700; }
.btn-logout {
  padding: 0.5rem 1.2rem; background: #f5f5f7; border: 1px solid #d2d2d7;
  border-radius: 8px; cursor: pointer; font-size: 0.85rem; color: #1d1d1f;
  transition: background 0.2s;
}
.btn-logout:hover { background: #e5e5e7; }
.stats-grid {
  display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem; margin-bottom: 2rem;
}
.admin-card {
  background: #fff; border: 1px solid #e5e5e7; border-radius: 12px;
  padding: 1.5rem; transition: box-shadow 0.2s;
}
.admin-card:hover { box-shadow: 0 4px 12px rgba(0,0,0,0.06); }
.admin-card h3 { font-size: 0.85rem; color: #86868b; font-weight: 500; margin-bottom: 0.5rem; }
.stat-num { font-size: 2rem; font-weight: 700; color: #1d1d1f; }
.section-title {
  font-size: 1.2rem; font-weight: 600; margin: 2rem 0 1rem;
  padding-bottom: 0.5rem; border-bottom: 1px solid #e5e5e7;
}
.log-list { list-style: none; }
.log-item {
  padding: 0.8rem 0; border-bottom: 1px solid #f0f0f2;
  font-size: 0.9rem; display: flex; justify-content: space-between; gap: 1rem;
}
.log-item:last-child { border-bottom: none; }
.log-msg { flex: 1; color: #333336; }
.log-time { color: #86868b; white-space: nowrap; font-size: 0.8rem; }
.action-grid {
  display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 1rem; margin-bottom: 2rem;
}
.action-btn {
  display: flex; align-items: center; gap: 0.8rem;
  background: #fff; border: 1px solid #e5e5e7; border-radius: 12px;
  padding: 1.2rem; cursor: pointer; transition: all 0.2s; text-decoration: none;
  color: #1d1d1f;
}
.action-btn:hover { box-shadow: 0 4px 12px rgba(0,0,0,0.08); transform: translateY(-1px); border-color: #0066cc; }
.action-icon { font-size: 1.5rem; }
.action-text h4 { font-size: 0.95rem; font-weight: 600; margin-bottom: 0.2rem; }
.action-text p { font-size: 0.8rem; color: #86868b; }
.modal-overlay {
  display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
  background: rgba(0,0,0,0.4); backdrop-filter: blur(10px); z-index: 200;
  align-items: center; justify-content: center; padding: 1rem;
}
.modal-overlay.active { display: flex; }
.modal-content {
  background: #fff; border-radius: 16px; padding: 2rem; width: 100%; max-width: 600px;
  max-height: 80vh; overflow-y: auto; box-shadow: 0 16px 48px rgba(0,0,0,0.15);
}
.modal-content h2 { font-size: 1.3rem; margin-bottom: 1.5rem; }
.modal-content label { display: block; font-size: 0.85rem; font-weight: 500; margin-bottom: 0.3rem; color: #86868b; }
.modal-content input, .modal-content textarea, .modal-content select {
  width: 100%; padding: 0.6rem 0.8rem; border: 1px solid #d2d2d7;
  border-radius: 8px; font-size: 0.9rem; margin-bottom: 1rem;
  font-family: inherit; outline: none;
}
.modal-content input:focus, .modal-content textarea:focus { border-color: #0066cc; }
.modal-content textarea { min-height: 300px; resize: vertical; }
.modal-actions { display: flex; gap: 0.8rem; justify-content: flex-end; margin-top: 1rem; }
.modal-actions button {
  padding: 0.6rem 1.5rem; border-radius: 8px; font-size: 0.9rem;
  font-weight: 500; cursor: pointer; border: none; transition: all 0.2s;
}
.btn-primary { background: #0066cc; color: #fff; }
.btn-primary:hover { background: #0077ed; }
.btn-secondary { background: #f5f5f7; color: #1d1d1f; border: 1px solid #d2d2d7 !important; }
.btn-secondary:hover { background: #e5e5e7; }
.btn-danger { background: #ff3b30; color: #fff; }
.btn-danger:hover { background: #ff453a; }
.loading { text-align: center; padding: 2rem; color: #86868b; }
.toast {
  position: fixed; bottom: 2rem; left: 50%; transform: translateX(-50%);
  background: #1d1d1f; color: #fff; padding: 0.8rem 1.5rem; border-radius: 10px;
  font-size: 0.9rem; z-index: 300; display: none;
  box-shadow: 0 4px 16px rgba(0,0,0,0.2);
}
.toast.show { display: block; animation: fadeInUp 0.3s ease; }
@keyframes fadeInUp { from { opacity: 0; transform: translate(-50%, 10px); } to { opacity: 1; transform: translate(-50%, 0); } }
</style>
</head>
<body>

<!-- Login -->
<div class="login-wrap" id="loginWrap">
  <div class="login-box">
    <h1>🔐</h1>
    <p>管理后台</p>
    <div class="login-error" id="loginError">密码错误</div>
    <input type="password" id="pwdInput" placeholder="输入密码" onkeydown="if(event.key==='Enter')doLogin()">
    <button onclick="doLogin()">登录</button>
  </div>
</div>

<!-- Admin Panel -->
<div class="admin-wrap" id="adminWrap">
  <div class="admin-header">
    <h1>⚙️ 管理后台</h1>
    <button class="btn-logout" onclick="doLogout()">退出</button>
  </div>

  <div class="stats-grid" id="statsGrid">
    <div class="admin-card"><h3>📄 文章数</h3><div class="stat-num" id="statPosts">-</div></div>
    <div class="admin-card"><h3>🖼️ 作品数</h3><div class="stat-num" id="statPortfolio">-</div></div>
    <div class="admin-card"><h3>💬 评论数</h3><div class="stat-num" id="statComments">-</div></div>
    <div class="admin-card"><h3>🔄 最近更新</h3><div class="stat-num" id="statLastUpdate" style="font-size:1rem;font-weight:400;margin-top:0.3rem;">-</div></div>
  </div>

  <h2 class="section-title">快捷操作</h2>
  <div class="action-grid">
    <div class="action-btn" onclick="openNewPost()">
      <div class="action-icon">✏️</div>
      <div class="action-text"><h4>新建文章</h4><p>创建新的博客文章</p></div>
    </div>
    <a class="action-btn" href="https://github.com/zach1219/zach1219.github.io/tree/main/_posts" target="_blank">
      <div class="action-icon">📂</div>
      <div class="action-text"><h4>管理文章</h4><p>在 GitHub 上查看所有文章</p></div>
    </a>
    <a class="action-btn" href="https://github.com/zach1219/zach1219.github.io/issues" target="_blank">
      <div class="action-icon">💬</div>
      <div class="action-text"><h4>管理评论</h4><p>查看 GitHub Issues 评论</p></div>
    </a>
    <a class="action-btn" href="https://github.com/zach1219/zach1219.github.io/actions" target="_blank">
      <div class="action-icon">🔄</div>
      <div class="action-text"><h4>构建状态</h4><p>查看 GitHub Actions 构建</p></div>
    </a>
    <a class="action-btn" href="/sitemap/" target="_blank">
      <div class="action-icon">🗺️</div>
      <div class="action-text"><h4>站点地图</h4><p>查看站点内容索引</p></div>
    </a>
    <a class="action-btn" href="https://github.com/zach1219/zach1219.github.io/settings/pages" target="_blank">
      <div class="action-icon">⚙️</div>
      <div class="action-text"><h4>Pages 设置</h4><p>GitHub Pages 配置</p></div>
    </a>
  </div>

  <h2 class="section-title">最近构建</h2>
  <ul class="log-list" id="buildLog">
    <li class="loading">加载中...</li>
  </ul>
</div>

<!-- New Post Modal -->
<div class="modal-overlay" id="newPostModal">
  <div class="modal-content">
    <h2>✏️ 新建文章</h2>
    <label>标题</label>
    <input type="text" id="postTitle" placeholder="文章标题">
    <label>标签（逗号分隔）</label>
    <input type="text" id="postTags" placeholder="标签1, 标签2">
    <label>正文（Markdown）</label>
    <textarea id="postBody" placeholder="在这里写 Markdown 内容..."></textarea>
    <div class="modal-actions">
      <button class="btn-secondary" onclick="closeModal('newPostModal')">取消</button>
      <button class="btn-primary" onclick="publishPost()">发布</button>
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
const GH_TOKEN = atob('Z2l0aHViX3BhdF8xMUFWTzZEVVEwR1lldmJudDBickJMX0Y2YjNWR3BSdk1nUDFDN2Fra0JIcFF1UkFTYW5QV2xNOGM0bkdrWkhtc2ZXRlBXUU5PSVhXTDl0dWsw');
const REPO = 'zach1219/zach1219.github.io';
const HEADERS = {'Authorization': 'Bearer ' + GH_TOKEN, 'Accept': 'application/vnd.github.v3+json'};

// Login
function doLogin() {
  const pwd = document.getElementById('pwdInput').value;
  if (pwd === 'BkttW7Fg8vnqgtb') {
    sessionStorage.setItem('yr_auth', '1');
    document.getElementById('loginWrap').style.display = 'none';
    document.getElementById('adminWrap').style.display = 'block';
    loadDashboard();
  } else {
    document.getElementById('loginError').style.display = 'block';
  }
}

function doLogout() {
  sessionStorage.removeItem('yr_auth');
  location.reload();
}

// Check auth on load
if (sessionStorage.getItem('yr_auth') === '1') {
  document.getElementById('loginWrap').style.display = 'none';
  document.getElementById('adminWrap').style.display = 'block';
  setTimeout(loadDashboard, 100);
}

// Load dashboard data
async function loadDashboard() {
  try {
    // Load posts
    const postsResp = await fetch('https://api.github.com/repos/' + REPO + '/contents/_posts', {headers: HEADERS});
    const posts = await postsResp.json();
    document.getElementById('statPosts').textContent = Array.isArray(posts) ? posts.length : 0;

    // Load portfolio
    const portResp = await fetch('https://api.github.com/repos/' + REPO + '/contents/_portfolio', {headers: HEADERS});
    const portfolio = await portResp.json();
    document.getElementById('statPortfolio').textContent = Array.isArray(portfolio) ? portfolio.length : 0;

    // Load issues (comments)
    const issuesResp = await fetch('https://api.github.com/repos/' + REPO + '/issues?labels=comment', {headers: HEADERS});
    const issues = await issuesResp.json();
    let commentCount = 0;
    issues.forEach(i => commentCount += (i.comments || 0));
    document.getElementById('statComments').textContent = commentCount;

    // Load recent builds
    const buildsResp = await fetch('https://api.github.com/repos/' + REPO + '/actions/runs?per_page=5', {headers: HEADERS});
    const builds = await buildsResp.json();
    const buildLog = document.getElementById('buildLog');
    if (builds.workflow_runs && builds.workflow_runs.length > 0) {
      document.getElementById('statLastUpdate').textContent = new Date(builds.workflow_runs[0].updated_at).toLocaleString('zh-CN');
      buildLog.innerHTML = builds.workflow_runs.map(run => {
        const status = run.conclusion === 'success' ? '✅' : run.conclusion === 'failure' ? '❌' : '⏳';
        const time = new Date(run.created_at).toLocaleString('zh-CN');
        return '<li class="log-item"><span class="log-msg">' + status + ' ' + run.head_commit.message.substring(0, 60) + '</span><span class="log-time">' + time + '</span></li>';
      }).join('');
    } else {
      buildLog.innerHTML = '<li class="log-item"><span class="log-msg">暂无构建记录</span></li>';
    }
  } catch(e) {
    console.error(e);
  }
}

// New Post
function openNewPost() {
  const today = new Date().toISOString().split('T')[0];
  document.getElementById('postTitle').value = '';
  document.getElementById('postTags').value = '';
  document.getElementById('postBody').value = '';
  document.getElementById('newPostModal').classList.add('active');
}

function closeModal(id) {
  document.getElementById(id).classList.remove('active');
}

async function publishPost() {
  const title = document.getElementById('postTitle').value.trim();
  const tags = document.getElementById('postTags').value.trim();
  const body = document.getElementById('postBody').value.trim();
  if (!title) { showToast('请输入标题'); return; }

  const today = new Date().toISOString().split('T')[0];
  const slug = title.replace(/[^\w\u4e00-\u9fff]/g, '-').replace(/-+/g, '-').substring(0, 60);
  const filename = today + '-' + slug + '.md';
  const tagList = tags ? tags.split(',').map(t => '  - ' + t.trim()).join('\n') : '  - 未分类';

  const content = '---\ntitle: \'' + title + '\'\ndate: ' + today + '\npermalink: /posts/' + today.replace(/-/g, '/') + '/' + slug + '/\ntags:\n' + tagList + '\n---\n\n' + body;

  try {
    const resp = await fetch('https://api.github.com/repos/' + REPO + '/contents/_posts/' + filename, {
      method: 'PUT',
      headers: HEADERS,
      body: JSON.stringify({
        message: 'feat: new post - ' + title,
        content: btoa(unescape(encodeURIComponent(content))),
        branch: 'main'
      })
    });
    if (resp.ok) {
      showToast('文章已发布: ' + filename);
      closeModal('newPostModal');
      loadDashboard();
    } else {
      showToast('发布失败: ' + resp.status);
    }
  } catch(e) {
    showToast('网络错误');
  }
}

function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 3000);
}
</script>
</body>
</html>
