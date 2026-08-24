---
title: Cursor规则与Copilot指令
date: 2026-08-11
tags:
  - AI指令文件
  - Cursor
  - GitHub Copilot
  - IDE
aliases:
  - Cursor 规则
  - Copilot 指令
  - mdc 规则
related:
  - "[[01-AI技术/Skill制作痛点/07_跨平台兼容性挑战|跨平台兼容性]]"
  - "[[01-AI技术/AI Agent 工具/AI_Agent_赛道一_终端CLI编程Agent|终端CLI编程Agent]]"
  - "[[01-AI技术/超长项目AI跑/02_上下文管理：AI的记忆体系|AI的记忆体系]]"
---

# Cursor 规则与 Copilot 指令

你在 Cursor 里写 `.cursor/rules`，补全效果很好；同事的仓库里规则文件躺在 `.github/instructions/`，Copilot 只认自己的路径。你把 Cursor 的规则原样复制过去，Copilot 却像没看见——两个工具的规则机制从触发方式到优先级完全不同。

这背后是两类工具的差别：Cursor 是 AI 原生 IDE，规则面向"对话与补全一体"的 Agent；Copilot 是补全插件，指令面向"每个请求都注入"的轻量约束。理解各自的加载机制，才能让指令文件真正被读到。

## Cursor 规则体系

Cursor 的规则放在 `.cursor/rules/*.mdc`，YAML frontmatter 控制触发方式，三个字段各管一条路：

- `alwaysApply: true`：每个会话必加载，适合放普适规则（代码风格、命令、禁止事项）。
- `globs: "src/**/*.tsx"`：匹配到相关文件时附加，适合放局部规则（某模块的约定）。
- `description`：AI 根据描述智能判断是否需要，适合放"场景化"规则。

三种触发按"确定性递减"排列。优先级从高到低：Team 规则 > Project 规则 > User 规则 > `.cursorrules` > `AGENTS.md`。

### 触发方式怎么选

| 这条规则的特征 | 用哪个字段 | 原因 |
| --- | --- | --- |
| 每个会话都要知道（代码风格、命令、禁区） | alwaysApply | 常驻加载，零判断成本 |
| 只对某类文件成立（tsx 组件、测试文件） | globs | 匹配才注入，省上下文 |
| 任务触发、路径表达不清（"写迁移脚本时"） | description | 让 AI 语义匹配 |

选错字段的代价很直接：alwaysApply 塞满规则，每个会话白白加载；globs 写得太宽等于 alwaysApply；description 依赖 AI 判断，关键时刻会漏。

> [!tip] 选择触发方式先问一句"这条规则是否每个会话都需要"。是，用 alwaysApply；只对特定目录成立，用 globs；遇到某类任务才需要的复杂规则，才靠 description 让 AI 自己判断——不要滥用，判断会失误。

### 团队共享与旧格式

Team 规则由团队统一维护（随仓库分发或通过 IDE 设置同步），优先级最高，适合放组织级红线，比如合规要求、禁用依赖。`.cursorrules` 是旧版单文件格式，功能上约等于 alwaysApply 的 `.mdc`；新项目直接用 `.cursor/rules/` 即可，历史遗留文件不必迁移，但要清楚它的优先级仍在 `.cursor/rules` 之下。

### 规则文件写作要点

- 单文件 ≤500 行，超过就拆：`.cursor/rules/frontend.mdc`、`.cursor/rules/testing.mdc`，按主题组合触发。
- 能引用文件就引用，别复制内容。规则里用路径指向现有文档，例如"测试约定见 docs/testing.md"，避免两处维护。
- 用 frontmatter 做元数据，正文只写规则本身，与 AGENTS.md 的纯 Markdown 风格保持一致，方便日后统一迁移。
- description 写一句话场景，别写长文：AI 靠它决定加载与否，写清楚"什么时候用"就够了。
- 同一事项只在一份规则里出现：两份规则写同一约定且不一致，AI 无所适从，这和 [[01-AI技术/Skill制作痛点/07_跨平台兼容性挑战|跨平台兼容性]] 里多平台写法互相打架是同一类问题。

### 模板

```markdown
---
description: 前端组件开发规则，打开 tsx 文件时生效
globs: "**/*.tsx"
alwaysApply: false
---

# 前端组件规则

- 组件用函数式写法，禁止 class 组件
- props 必须显式声明类型
- 状态提升到最近的共同父组件
- 样式用 Tailwind，禁止内联 style
```

### 组合使用示例

一个中型仓库的规则布局：

```
.cursor/rules/
├── global.mdc        # alwaysApply：技术栈、命令、编码红线
├── frontend.mdc      # globs: **/*.tsx：组件与样式约定
├── backend.mdc       # globs: **/*.ts：接口与数据层约定
└── migration.mdc     # description：数据库迁移类任务才加载
```

打开一个 tsx 文件时，AI 拿到 global 加 frontend 两份；写后端接口时换成 global 加 backend；做迁移时三份全上。每份都短，组合起来才完整。判断拆分是否合理，看一个指标：改动一个前端约定时，是否只需要动 frontend.mdc 一个文件。

### 验证规则生效

写规则后必须验证，别写完就当完成：

- 在 Cursor 的 Rules 面板里逐条查看触发方式与优先级顺序。
- 新开会话，问"本仓库的代码约定是什么"，看 AI 是否说出规则原文。
- 打开一个 tsx 文件触发 globs，再问"这个文件要遵守什么约定"，确认附加规则生效。

## Copilot 指令体系

GitHub Copilot 分两个层级：

**仓库级**：`.github/copilot-instructions.md`，自动注入到每个请求，建议不超过两页。放最通用的约束：语言、框架、测试框架、提交规范。

**路径级**：`.github/instructions/NAME.instructions.md`，用 `applyTo` 声明匹配路径，只有相关文件出现时才注入：

```markdown
---
applyTo: "src/**/*.ts"
---

# TypeScript 约定
- 使用严格模式
- 类型别名优先于接口
```

路径级指令适合"仓库内分模块约定"：后端目录放一套、前端目录放另一套，互不干扰。

仓库级指令里该放什么：语言与框架、构建与测试命令、提交规范、安全红线。别放的内容：会话进度、一次性任务说明、个人偏好。两页是硬约束，超了就拆到路径级指令，或者放回 AGENTS.md 让所有工具共享。

路径级指令用 `applyTo` 声明范围，支持 glob，一个仓库可以按目录建多份。命名建议 NAME 与目录对应，比如 `api.instructions.md`、`web.instructions.md`，看文件名就知道作用范围，维护时不迷路。

> [!tip] Copilot 是"注入式"机制，指令每次请求都占用 token，所以它天然倾向极简。仓库级指令放"所有请求都该知道的事"，其余一律走路径级。

## 与 AGENTS.md 的优先级

Cursor 把 AGENTS.md 放在优先级最底，作为兜底；Copilot 的仓库级指令与 AGENTS.md 并存，同一事项上具体指令覆盖通用指令。

实际排位大致是：Cursor/Copilot 自己的规则 > AGENTS.md > 默认行为。这意味着：

- 工具专属规则（UI 交互习惯、补全偏好）留在工具规则文件。
- 仓库级普适事实（技术栈、命令）放 AGENTS.md，所有工具共享。
- 两者冲突时以工具规则文件为准，所以别在两边写矛盾内容。

举个具体例子：AGENTS.md 写"缩进用 2 空格"，Cursor 规则写"本仓库用 4 空格"，生效的是 Cursor 规则；AGENTS.md 写了而 Cursor 规则没写的内容，Cursor 规则不覆盖，AGENTS.md 兜底。所以排查"AI 为什么没按规矩来"时，先看是不是某一层规则把 AGENTS.md 盖住了。

这套"专属优先、通用兜底"的排位，和 [[01-AI技术/超长项目AI跑/02_上下文管理：AI的记忆体系|AI的记忆体系]] 里"近层覆盖远层"的原则一致：离任务越近的规则越有发言权。IDE 侧规则的加载量都有限，[[01-AI技术/Skill制作痛点/07_跨平台兼容性挑战|跨平台兼容性]] 的教训同样适用：能在 AGENTS.md 里通用的内容，别在每家工具里各写一份。

## 现在你应该怎么做

1. 在 `.cursor/rules/` 建两到三个文件：一个 `alwaysApply` 放普适规则，其余用 `globs` 按目录或文件类型触发。
2. 仓库根已有 AGENTS.md 的话，Cursor 侧只保留工具特有内容，普适内容不重复写。
3. 在 `.github/copilot-instructions.md` 写仓库级指令，控制在两页内。
4. 有分模块约定的仓库，再建 `.github/instructions/*.instructions.md` 用 `applyTo` 分流。
5. 分别用 Cursor 和 Copilot 各开一次会话验证规则生效，再从工具特有细节里提炼出可迁移到 AGENTS.md 的通用事实，反哺统一文件。
