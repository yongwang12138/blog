+++
date = '2026-03-19T18:40:00+08:00'
draft = true
title = 'Hugo 博客运行与发布指南'
tags = ['hugo', 'git', '部署']
categories = ['博客维护']
+++

本文记录这个博客项目的常用操作：本地运行、构建、以及推送发布。

## 1. 环境准备

需要先安装：

- Git
- Hugo Extended（建议版本不低于 v0.146.0）

检查版本：

```bash
git --version
hugo version
```

## 2. 拉取项目

首次拉取（包含主题子模块）：

```bash
git clone --recurse-submodules <你的仓库地址>
cd notes
```

如果你已经拉过仓库，但主题目录为空或没更新：

```bash
git submodule update --init --recursive
```

更新主题子模块到远端最新提交：

```bash
git submodule update --remote --merge
```

## 3. 本地运行预览

在项目根目录运行：

```bash
hugo server -D
```

说明：

- 默认访问地址：`http://localhost:1313`
- `-D` 表示包含 `draft = true` 的草稿文章
- 修改 `content/` 下文档后会自动热更新

## 4. 新增文章

方式一：手动创建目录与 `index.md`（当前项目常用）

```text
content/posts/<文章slug>/index.md
```

方式二：使用 Hugo 命令生成骨架

```bash
hugo new content/posts/my-post/index.md
```

然后把 Front Matter 里的 `draft` 改为 `false` 才会出现在正式站点。

## 5. 构建静态文件

构建生产版本：

```bash
hugo --minify
```

构建结果在：

```text
public/
```

## 6. 推送与发布

日常提交流程：

```bash
git add .
git commit -m "docs: 更新博客内容"
git push origin main
```

如果你改了主题子模块指针（`themes/PaperMod`），也需要一起提交。

是否自动发布取决于你的托管平台：

- GitHub Pages / Netlify / Vercel：通常在 `push` 后自动构建发布
- 自建服务器：通常需要把 `public/` 同步到服务器站点目录

## 7. 常见问题

### 7.1 主题丢失或样式异常

执行：

```bash
git submodule update --init --recursive
```

### 7.2 页面不显示新文章

按顺序检查：

- Front Matter 的 `draft = true`
- 文章日期是否正确
- 本地预览是否使用了 `hugo server -D`

### 7.3 构建报 Hugo 版本过低

升级 Hugo Extended 到主题要求版本后重试。
