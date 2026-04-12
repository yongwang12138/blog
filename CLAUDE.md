# Claude 协作指南

这是一个基于 Hugo 的中文技术博客项目，主题为 Mars星球。

## 项目概述

- **站点地址**: https://yongwang12138.github.io/blog/
- **博客标题**: Mars星球
- **语言**: 中文 (zh-cn)
- **时区**: Asia/Shanghai
- **静态站点生成器**: Hugo

## 项目结构

```
blog/
├── content/          # 博客内容（Markdown 文件）
│   ├── cpp/         # C++ 相关文章
│   ├── git/         # Git 相关文章
│   └── other/       # 其他分类文章
├── layouts/         # Hugo 模板文件
├── static/          # 静态资源（图片、CSS 等）
├── public/          # 构建输出目录（不提交到 Git）
├── archetypes/      # 内容模板
├── data/            # 数据文件
├── i18n/            # 国际化配置
└── hugo.toml        # Hugo 配置文件
```

## 内容组织

### 文章分类

博客按以下分类组织内容：

- **C++**: C++ 编程相关文章
- **Git**: Git 版本控制教程
- **其他**: 其他技术主题

### 永久链接格式

文章使用 `/:section/:slug.html` 格式，例如：
- `/cpp/cpp-header-only-libs.html`
- `/git/git-tutorial.html`
- `/other/compare.html`

## 开发工作流

### 本地开发

```bash
# 启动开发服务器
hugo server -D

# 构建站点
hugo

# 创建新文章
hugo new cpp/my-article.md
```

### 部署

项目通过 GitHub Actions 自动部署到 GitHub Pages。推送到 main 分支会触发自动构建和部署。

## 编辑内容时的注意事项

1. **Markdown 格式**: 所有文章使用 Markdown 编写，支持原生 HTML（`unsafe = true`）
2. **中文优化**: 已启用 `hasCJKLanguage = true`，摘要和字数统计针对中文优化
3. **图片路径**: 静态资源放在 `static/` 目录，引用时使用 `/` 开头的绝对路径
4. **Front Matter**: 新文章应包含标题、日期、分类等元数据

### 文章模板示例

```markdown
---
title: "文章标题"
date: 2026-04-12
draft: false
categories: ["cpp"]
tags: ["C++", "编程"]
---

文章内容...
```

## 配置修改指南

### 修改站点信息

编辑 `hugo.toml` 中的以下字段：
- `title`: 博客标题
- `baseURL`: 站点地址
- `params.description`: 站点描述

### 修改导航菜单

在 `hugo.toml` 的 `[menu.home]` 部分添加或修改导航项。

### 修改首页内容

首页的 Hero 区、特性卡片、按钮组都在 `hugo.toml` 的 `[params.home]` 部分配置。

## Git 工作流

- **主分支**: main
- **提交信息**: 使用中文描述变更内容
- **不要提交**: `public/` 目录（构建产物）和 `.hugo_build.lock`

## 常见任务

### 添加新文章

1. 在 `content/` 对应分类目录下创建 Markdown 文件
2. 添加 Front Matter 元数据
3. 编写文章内容
4. 本地预览确认无误
5. 提交并推送到 GitHub

### 修改样式

自定义 CSS 文件位于 `static/home.css`，可以覆盖主题默认样式。

### 添加静态资源

将图片、字体等资源放入 `static/` 目录，Hugo 会将其复制到 `public/` 根目录。

## 技术栈

- **Hugo**: 静态站点生成器
- **Markdown**: 内容编写格式
- **GitHub Pages**: 托管平台
- **GitHub Actions**: CI/CD 自动部署

## 注意事项

- 修改配置文件后需要重启 Hugo 服务器
- 图片文件应优化大小以提升加载速度
- 文章 slug 应使用英文，避免中文 URL
- 提交前确保本地构建成功（`hugo` 命令无错误）
