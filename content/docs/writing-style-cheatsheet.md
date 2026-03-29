+++
title = "写作样式速查"
description = "博客常用 Markdown 样式与可复制模板"
+++

# 写作样式速查

这份文档记录当前博客可直接使用的常见样式，复制即可。

## 1. 提示块（TIP）

> **TIP**
> 这里写提示内容。

可复制模板：

```md
> **TIP**
> 这里写提示内容。
```

## 2. 注意块（NOTE）

> **NOTE**
> 这里写补充说明。

可复制模板：

```md
> **NOTE**
> 这里写补充说明。
```

## 3. 警告块（WARNING）

> **WARNING**
> 这里写风险提醒。

可复制模板：

```md
> **WARNING**
> 这里写风险提醒。
```

## 4. 代码块

```bash
# 安装依赖
npm install

# 启动开发
npm run dev
```

可复制模板：

````md
```bash
# 你的命令
```
````

常用语言标记：`bash`、`cpp`、`go`、`js`、`ts`、`json`、`yaml`、`toml`。

## 5. 行内代码

比如运行 `hugo -D` 启动本地预览。

可复制模板：

```md
比如运行 `hugo -D` 启动本地预览。
```

## 6. 折叠块（details）

<details>
<summary>点击展开</summary>

这里写隐藏内容。

- 支持列表
- 支持代码块

</details>

可复制模板：

```md
<details>
<summary>点击展开</summary>

这里写隐藏内容。

</details>
```

## 7. 表格

| 名称 | 说明 |
| --- | --- |
| Hugo | 静态站点生成器 |
| Markdown | 写作格式 |

可复制模板：

```md
| 名称 | 说明 |
| --- | --- |
| A | B |
```

## 8. 任务列表

- [x] 已完成
- [ ] 待完成

可复制模板：

```md
- [x] 已完成
- [ ] 待完成
```

## 9. 图片

![示例图片](/blog/background.svg)

可复制模板：

```md
![示例图片](/blog/你的图片路径)
```

## 10. 链接

[GitHub](https://github.com/yongwang12138)

可复制模板：

```md
[链接文字](https://example.com)
```

---

## 推荐写法

1. 小提示统一用 `> **TIP**`。
2. 命令统一用 `bash` 代码块。
3. 长内容用 `<details>` 折叠，保持页面清爽。
4. 每篇文章保持 1 个 H1，其他用 H2/H3 分层。
