+++
date = '2026-03-09T20:31:36+08:00'
draft = false
title = 'Git基础教程'
tags = ['git', '开发工具']
categories = ['开发环境']
+++

# Git 从零到协作实用指南

这篇文档按实际工作顺序整理：`初始化仓库 -> 连接远程仓库 -> 首次推送 -> 日常提交 -> 版本标签 -> 冲突处理`。

## 1. 首次使用前的基础配置

### 1.1 配置用户名和邮箱

Git 的每次提交都会记录作者信息，建议先全局配置一次。

```bash
git config --global user.name "wangyongwang"
git config --global user.email "13327418643@163.com"
```

查看当前配置：

```bash
git config --global user.name
git config --global user.email
```

{{< notice type="tip" >}}
如果某个项目需要单独身份，可以在项目目录里去掉 `--global` 再设置一次。
{{< /notice >}}

### 1.2 多平台 SSH 配置（GitHub / Gitee / GitLab）

如果你要同时连多个代码平台，建议给每个平台单独一把密钥，再用 `~/.ssh/config` 做映射。

#### 1.2.1 生成密钥（推荐 ED25519）

```bash
ssh-keygen -t ed25519 -f ~/.ssh/github -C "办公电脑"
ssh-keygen -t ed25519 -f ~/.ssh/gitee -C "办公电脑"
ssh-keygen -t ed25519 -f ~/.ssh/gitlab_45_211 -C "办公电脑"
ssh-keygen -t ed25519 -f ~/.ssh/gitlab_18_253 -C "办公电脑"
```

#### 1.2.2 配置 `~/.ssh/config`

```ini
# ========== GitHub ==========
Host github.com
HostName github.com
User git
IdentityFile ~/.ssh/github
PubkeyAcceptedKeyTypes ssh-ed25519

# ========== Gitee ==========	
Host gitee.com
HostName gitee.com
User git
IdentityFile ~/.ssh/gitee
PubkeyAcceptedKeyTypes ssh-ed25519
	
# ========== 公司 GitLab 188.18.45.211 ==========
Host 188.18.45.211
HostName 188.18.45.211
User git
IdentityFile ~/.ssh/gitlab_45_211
PubkeyAcceptedKeyTypes ssh-ed25519
	
# ========== 公司 GitLab 188.18.18.253 ==========
Host 188.18.18.253
HostName 188.18.18.253
User git
IdentityFile ~/.ssh/gitlab_18_253
PubkeyAcceptedKeyTypes ssh-ed25519
```

#### 1.2.3 测试是否连通

```bash
ssh -T git@github.com
ssh -T git@gitee.com
ssh -T git@188.18.45.211
```

简单理解：

- 每个平台用不同密钥，更安全，也更容易排查问题。
- `Host` 是别名，`git clone`/`git push` 时会按别名找到对应私钥。

## 2. 新项目从 0 创建 Git 仓库

### 2.1 在项目目录初始化

```bash
git init
git branch -m main
```

简单理解：

- `git init`：把当前目录变成 Git 仓库。
- `git branch -m main`：把当前分支改名为 `main`，后续和大多数远程仓库默认分支保持一致。

### 2.2 第一次本地提交

```bash
git add .
git commit -m "feat: 初始化项目"
```

简单理解：

- `git add .`：把当前目录下的改动加入暂存区。
- `git commit -m "..."`：形成一个本地版本快照。

## 3. 连接远程仓库并首次推送

假设远程地址是你给出的：

```bash
git remote add origin git@188.18.45.211:wangyongwang/record-manager-agent.git
```

检查远程是否添加成功：

```bash
git remote -v
```

首次推送建议用：

```bash
git push -u origin main
```

简单理解：

- `origin`：远程仓库别名（默认常用名）。
- `-u`：把本地 `main` 与远程 `origin/main` 建立跟踪关系。
- 之后可直接用 `git push` 和 `git pull`，不用每次写完整分支名。

{{< notice type="tip" >}}
如果远程仓库里已经有初始化文件（如 README），首次推送前先执行 `git pull --rebase origin main`，避免被拒绝推送。
{{< /notice >}}

## 4. 日常开发最常用流程

### 4.1 提交本地修改

```bash
git status
git add .
git commit -m "feat: 新增某功能"
```

### 4.2 推送到远程

```bash
git push
```

### 4.3 开发前先同步远程

```bash
git pull --rebase
```

简单理解：

- `git status`：先看工作区是否干净、有哪些文件改了。
- `git pull --rebase`：先拿到远程最新提交，再把你的提交“接”到后面，历史更直。

## 5. 版本标签（发布用）

查看已有标签：

```bash
git tag
```

创建带说明的标签：

```bash
git tag -a v1.0.0 -m "发布 1.0.0 版本"
```

推送单个标签：

```bash
git push origin v1.0.0
```

推送全部标签：

```bash
git push origin --tags
```

简单理解：

- 标签常用于“可回溯发布点”，例如上线版本 `v1.0.0`。

## 6. Git 冲突处理（协作重点）

常见场景：你和同事改了同一文件同一区域，拉取或合并时会冲突。

### 6.1 推荐处理步骤

```bash
# 1) 先保存当前未提交改动
git stash

# 2) 拉最新代码
git pull --rebase

# 3) 还原自己的改动
git stash pop
```

如果出现冲突标记（如 `<<<<<<<`、`=======`、`>>>>>>>`）：

```bash
# 4) 手动编辑冲突文件后
git add .

# 5) 完成这次冲突处理
git rebase --continue

# 6) 最后推送
git push
```

{{< notice type="warning" >}}
冲突本质是“同一位置有两份不同修改”。保留正确代码并删除冲突标记后，才能继续提交。
{{< /notice >}}

## 7. 常见检查命令速查

```bash
git log --oneline --graph --decorate -20
git branch
git remote -v
git status
```

简单理解：

- `git log --oneline --graph --decorate -20`：快速看最近提交和分支走向。
- `git branch`：看本地分支。
- `git remote -v`：看远程地址。
- `git status`：看当前改动状态。

---

## 8. Git 双远程开发全流程（从官方仓库开始）

你的目标可以抽象成 3 件事：

1. 先拉官方仓库代码（以 `picoclaw` 为例）。
2. 在本地开发和提交。
3. 把代码放到你自己的 GitLab 仓库，并长期跟进官方更新。

这里统一用这两个远程地址示例：

- 官方仓库（只读同步）：`https://github.com/sipeed/picoclaw.git`
- 你的仓库（可写推送）：`git@188.18.45.211:wangyongwang/picoclaw.git`

远程命名约定：

- `upstream`：官方仓库
- `origin`：你的 GitLab 仓库

### 8.1 第一步：先克隆官方代码

```bash
git clone https://github.com/sipeed/picoclaw.git
cd picoclaw
```

克隆官方仓库后，默认只有一个远程，名字通常是 `origin`，但它现在指向官方仓库。  
你后续要推到自己的 GitLab，所以要把远程关系调整成“双远程”。

### 8.2 第二步：把远程改成双远程结构

```bash
# 把当前 origin（官方）改名为 upstream
git remote rename origin upstream

# 添加你自己的 GitLab 作为 origin
git remote add origin git@188.18.45.211:wangyongwang/picoclaw.git

# 检查结果
git remote -v
```

你应看到类似：

- `origin` -> `git@188.18.45.211:wangyongwang/picoclaw.git`
- `upstream` -> `https://github.com/sipeed/picoclaw.git`

### 8.3 第三步：在 GitLab 上创建空仓库后，首次推送 main

你在 GitLab 网页创建好空仓库（不要勾选 README 初始化）后，执行：

```bash
git checkout main
git push -u origin main
```

`-u` 的意义：把本地 `main` 关联到远程 `origin/main`，以后直接 `git push` 即可。

### 8.4 第四步：开始开发（永远在功能分支上改）

```bash
# 从 main 拉一个功能分支
git checkout -b feat/xxx

# 开发后提交
git add .
git commit -m "feat: xxxxx"

# 推送你的功能分支到 GitLab
git push -u origin feat/xxx
```

为什么不直接改 `main`：  
这样可以让主分支保持稳定，合并、回滚、代码评审都更清晰。

### 8.5 第五步：持续同步官方最新代码

官方更新后，建议固定执行这套动作：

```bash
# 取回官方仓库最新提交
git fetch upstream

# 切到本地 main，并把它对齐官方 main
git checkout main
git rebase upstream/main

# 把同步后的 main 推到你自己的 GitLab
git push origin main
```

简单理解：`upstream` 是“信息源”，`origin` 是“你的工作仓库”。

### 8.6 第六步：让你的功能分支跟上最新 main

```bash
git checkout feat/xxx
git rebase main

# 如果没有冲突，直接更新远程分支
git push --force-with-lease origin feat/xxx
```

如果有冲突：

```bash
# 1) 手动改冲突文件后
git add .

# 2) 继续 rebase
git rebase --continue

# 3) 全部完成后再推送
git push --force-with-lease origin feat/xxx
```

为什么这里要 `--force-with-lease`：  
`rebase` 会改写提交历史，普通 `push` 会被拒绝；`--force-with-lease` 比 `--force` 更安全。

### 8.7 第七步：合并与收尾

在 GitLab 发起 MR（`feat/xxx` -> `main`），合并后本地清理：

```bash
git checkout main
git pull --rebase origin main

git branch -d feat/xxx
git push origin --delete feat/xxx
```

### 8.8 新电脑接着开发怎么做

```bash
git clone git@188.18.45.211:wangyongwang/picoclaw.git
cd picoclaw

# 补上官方远程
git remote add upstream https://github.com/sipeed/picoclaw.git

git fetch --all
git checkout -b feat/xxx origin/feat/xxx
```

### 8.9 常见坑与处理

1. `origin` / `upstream` 配反了：

```bash
git remote -v
git remote set-url origin git@188.18.45.211:wangyongwang/picoclaw.git
git remote set-url upstream https://github.com/sipeed/picoclaw.git
```

2. `push` 被拒绝（远程分支更新了）：

```bash
git pull --rebase origin <branch>
git push origin <branch>
```

3. 不小心在 `main` 上开发了：

```bash
git checkout -b feat/rescue-main-work
git checkout main
git reset --hard origin/main
```

第 3 条是“紧急修正流程”，`reset --hard` 会丢弃当前分支未保存改动，务必先确认代码已经在 `feat/rescue-main-work` 里。
