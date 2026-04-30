# Mars星球博客

一个基于 Hugo 的静态博客，使用 DoIt 主题。

## 环境要求

- [Hugo Extended 版本](https://github.com/gohugoio/hugo/releases) (推荐 0.100.0 或更高)
- Git

## 快速开始

### 1. 安装 Hugo

**Windows (使用 Chocolatey):**
```bash
choco install hugo-extended -y
```

**Windows (使用 Scoop):**
```bash
scoop install hugo-extended
```

**macOS (使用 Homebrew):**
```bash
brew install hugo
```

**Linux:**
```bash
# Ubuntu/Debian
sudo apt-get install hugo

# Arch Linux
sudo pacman -S hugo
```

验证安装：
```bash
hugo version
```

### 2. 克隆项目

```bash
git clone https://github.com/yongwang12138/blog.git
cd blog
```

### 3. 启动本地开发服务器

```bash
hugo server -D
```

`-D` 参数会包含草稿文章。如果只想看已发布的文章，去掉 `-D`。

启动成功后，在浏览器中访问：http://localhost:1313/blog/

### 4. 构建静态网站

生成静态文件到 `public` 目录：

```bash
hugo
```

自定义输出目录：
```bash
hugo -d dist
```

## 项目结构

```
blog/
├── content/          # 博客文章内容
│   ├── cpp/         # C++ 相关文章
│   ├── git/         # Git 相关文章
│   └── other/       # 其他文章
├── layouts/         # 自定义布局模板
├── static/          # 静态资源（图片、CSS 等）
├── archetypes/      # 文章模板
├── hugo.toml        # Hugo 配置文件
└── README.md        # 项目说明文档
```

## 写新文章

### 创建新文章

```bash
hugo new content/cpp/my-new-post.md
```

### 文章格式

文章使用 Markdown 格式，头部包含 Front Matter：

```yaml
---
title: "文章标题"
date: 2026-04-30T14:00:00+08:00
categories: ["教程"]
tags: ["Git", "基础"]
draft: false  # true 为草稿，false 为发布
slug: "custom-url-slug"  # 可选，自定义 URL
---
```

### 文章目录结构

- `content/cpp/` - C++ 相关文章
- `content/git/` - Git 相关文章
- `content/other/` - 其他主题文章

## 常用命令

```bash
# 启动开发服务器（包含草稿）
hugo server -D

# 启动开发服务器（草稿可见，且支持实时刷新）
hugo server -D --disableFastRender

# 构建生产版本
hugo

# 构建并最小化输出
hugo --minify

# 查看 Hugo 版本
hugo version

# 清理缓存
hugo --gc
```

## 配置说明

主要配置在 `hugo.toml` 文件中：

- `baseURL` - 站点基础 URL
- `title` - 博客标题
- `theme` - 使用的主题（如果没有单独设置，需要在 `themes/` 目录中添加主题）
- `menu` - 导航菜单配置

## 部署

### 部署到 GitHub Pages

1. 构建网站：
```bash
hugo
```

2. 将 `public` 目录内容推送到 GitHub Pages 分支：
```bash
cd public
git add .
git commit -m "Update site"
git push origin main
```

或使用 `deploy.sh` 脚本自动化部署。

## 故障排除

### 1. 主题未找到

如果看到 "theme not found" 错误，需要安装主题：

```bash
git submodule add https://github.com/HEIGE-HEIGE/hugo-theme-docs.git themes/doit
```

### 2. 页面样式丢失

检查 `baseURL` 配置是否正确。如果部署到 GitHub Pages 子路径，确保 `baseURL` 包含子路径。

### 3. 本地服务器不自动刷新

使用 `--disableFastRender` 参数启动服务器：
```bash
hugo server -D --disableFastRender
```

## 相关资源

- [Hugo 官方文档](https://gohugo.io/documentation/)
- [Hugo 主题列表](https://themes.gohugo.io/)
- [Markdown 语法指南](https://www.markdownguide.org/)

## 许可证

查看 LICENSE 文件了解详细信息。
