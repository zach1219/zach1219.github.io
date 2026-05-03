# zach1219.github.io

个人博客与作品集站点，基于 [Jekyll](https://jekyllrb.com/) 构建，托管于 [GitHub Pages](https://pages.github.com/)。

🔗 **在线访问**：[https://zach1219.github.io](https://zach1219.github.io)

## 站点结构

| 板块 | 说明 |
|------|------|
| **Memoir** | 回忆录 / 博客文章 |
| **Gallery** | 作品展示 |
| **CV** | 个人简历 |

## 技术栈

- **框架**：Jekyll
- **主题**：Academic Pages (air)
- **托管**：GitHub Pages
- **评论**：Staticman（可选）

## 本地开发

```bash
# 安装依赖
bundle install

# 启动本地服务器
bundle exec jekyll serve
```

访问 `http://localhost:4000` 预览。

## 内容管理

- 博客文章放在 `_posts/` 目录
- Gallery 作品放在 `_portfolio/` 目录
- 导航菜单在 `_data/navigation.yml` 配置
- 站点参数在 `_config.yml` 配置

## License

MIT