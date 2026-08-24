---
title: Git 极简生存指南 — 只学 5 分钟就能用的版本控制
date: 2026-07-25
tags:
  - vibe-coding
  - Git
  - 版本控制
  - 入门
aliases:
  - Git指南
  - 版本控制
  - git教程
audience: 专业版
status: done
---

# Git 极简生存指南 — 只学 5 分钟就能用的版本控制

> [!abstract] 这篇解决什么问题
> Git 是 Vibe Coder 最重要的安全网——==5 分钟学会，每次迭代省 2 小时。== 不需要学分支、merge、rebase，只需要学会「存档」和「读档」。
>
> 前置阅读：[[05-产品与开发/Vibe Coder/Vibe Coder 常见问题全景图#2.3 改了 A 坏了 B——迭代地狱 ♻️]]

---

## 一、用游戏存档理解 Git

| 游戏 | Git |
|---|---|
| 打 Boss 前存档 | 让 AI 改代码前 `git commit` |
| 死了 → 读档重来 | AI 改坏了 → `git checkout .` 回退 |
| 多个存档位 | 每次 `commit` 都是一个存档点 |
| 可以看到每个存档的备注 | `git log` 看提交历史 |

> [!tip] 你只需要把 Git 当成 ==「代码的 Ctrl+S + 时间机器」==。

---

## 二、5 分钟初始化（只需做一次）

```bash
# 1. 进入项目文件夹
cd 你的项目文件夹路径

# 2. 初始化（创建一个隐藏的 .git 文件夹，用来存所有版本）
git init

# 3. 让 AI 帮你创建 .gitignore
# 对 AI 说：「帮我创建 .gitignore 文件，忽略 node_modules、.env、dist」
```

### .gitignore 是什么？

它是一个「黑名单」——告诉 Git 哪些文件/文件夹不用管。你只需要忽略 `node_modules`（太大）和 `.env`（含密钥）。

---

## 三、你只需要两条命令

### 存档：`git add . && git commit -m "备注"`

```bash
# 每次 AI 改完代码、你测试通过后执行
git add . && git commit -m "v1.2 完成了用户登录功能"
```

| 部分 | 含义 |
|---|---|
| `git add .` | 把所有修改加入「待存档列表」 |
| `git commit -m "..."` | 正式存档，备注写清楚这次改了什么 |
| `&&` | 两条命令一起执行 |

### 读档：`git checkout .`

```bash
# AI 改坏了，想回到上一次存档的状态
git checkout .
```

> [!warning] `git checkout .` 会丢弃所有未存档的修改。执行前确认：你真的要放弃当前所有改动吗？

---

## 四、你可能还需要知道的三条命令

### 查看状态：`git status`

```bash
git status
```

显示：哪些文件被修改了、哪些文件是新加的、哪些文件还没存档。

### 查看历史：`git log --oneline`

```bash
git log --oneline
```

显示所有存档点（从新到旧），每个一行。用于回顾「我之前做了什么」。

### 回到某个存档：`git checkout <commit-id>`

```bash
# 先用 git log --oneline 找到你想回到的存档 ID
git log --oneline
# 输出：
# a1b2c3d v1.3 新增搜索功能
# e4f5g6h v1.2 完成登录功能
# i7j8k9l 初始版本

# 回到 v1.2
git checkout e4f5g6h
```

> [!danger] 谨慎使用
> 这会进入「游离状态」。通常你只需要 `git checkout .`（回到最新存档），不需要回到历史存档。

---

## 五、Vibe Coding 场景实战

### 场景 1：正常迭代

```bash
# AI 完成了登录功能，你测试通过
git add . && git commit -m "v1.1 完成用户登录功能"

# AI 加了一个搜索功能，你测试通过
git add . && git commit -m "v1.2 新增全局搜索功能"

# AI 加了一个导出功能，你测试通过
git add . && git commit -m "v1.3 新增数据导出功能"
```

### 场景 2：AI 改坏了，回退

```bash
# AI 在加搜索功能时把登录搞崩了
# 你测试发现登录页面打不开了

# 回退到上一个稳定版本
git checkout .

# 项目回到 v1.1 的状态（登录功能正常，搜索功能没了）
# 重新让 AI 加搜索功能，这次加上 Prompt 约束
```

### 场景 3：想看 AI 改了哪些文件

```bash
# 在 commit 之前，看看 AI 到底动了哪些文件
git status

# 看具体改了什么内容
git diff
```

---

## 六、Git 和 GitHub 的区别

| | Git | GitHub |
|---|---|---|
| 是什么 | 你电脑上的版本管理工具 | 网上的代码托管平台 |
| 需要联网吗 | 不需要 | 需要 |
| 你需要学吗 | ==必须学== | 暂时不用 |

> [!tip] 对 Vibe Coder 来说，Git 是必须的，GitHub 是可选的。先学会 Git 本地存档，等项目需要多人协作或部署时再学 GitHub。

---

## 七、常见问题

### Q：`git commit` 报错「Author identity unknown」怎么办？

> 首次使用 Git 需要设置用户名和邮箱（只是标识，不需要真实信息）：
> ```bash
> git config --global user.name "你的名字"
> git config --global user.email "你的邮箱@example.com"
> ```

### Q：我忘了有没有 commit 过，怎么确认？

> ```bash
> git log --oneline
> ```
> 如果有输出，说明有存档。如果显示「fatal: not a git repository」，说明还没初始化。

### Q：commit 之后还能撤销吗？

> 可以，但比较复杂。对新手来说，==commit 之前先用 `git status` 确认改了哪些文件==，确认无误再 commit。如果已经 commit 了想撤销，问 AI：「帮我撤销最近一次 git commit，但保留代码改动。」

### Q：`.git` 文件夹可以删掉吗？

> 删掉 `.git` 文件夹 = 所有存档记录都没了（但代码文件还在）。==不要手动删。==

---

## 八、一句话总结

> [!tip] Git = `git add . && git commit -m "备注"` + `git checkout .`
> 每次迭代前存档，AI 改坏了就回退。5 分钟学会，终身受益。

---

## 关联笔记

- [[05-产品与开发/Vibe Coder/02-开发实战/Vibe Coding 迭代防退化指南]] — 迭代防退化的完整防御体系
- [[05-产品与开发/Vibe Coder/Vibe Coder 常见问题全景图]] — 回到总索引
- [[05-产品与开发/Vibe Coder/AI生成产品的经验法则]] — 经验 8：遇到报错不要慌
- [[05-产品与开发/Vibe Coder/Vibe Coding 软件瘦身指南]] — 交付给用户的正确姿势