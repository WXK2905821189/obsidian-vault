---
title: CLAUDE.md编写规范
date: 2026-08-11
tags:
  - AI指令文件
  - CLAUDE.md
  - Claude Code
  - 上下文工程
aliases:
  - CLAUDE.md
  - Claude Code 记忆文件
  - 项目指令文件
related:
  - "[[01-AI技术/AI Agent 工具/AI_Agent_赛道一_终端CLI编程Agent|终端CLI编程Agent]]"
  - "[[05-产品与开发/AI驱动Web应用开发/02-文档体系篇/02_项目记忆文档|项目记忆文档]]"
  - "[[01-AI技术/超长项目AI跑/02_上下文管理：AI的记忆体系|AI的记忆体系]]"
  - "[[01-AI技术/LOOP Engineering/03-架构篇/03_Hook Loop Harness范式|Hook Loop Harness]]"
---

# CLAUDE.md 编写规范

你用 Claude Code 写了一个记账应用。第一天开新会话，AI 反问"这是什么项目"，你把背景从头讲一遍；半个月后 CLAUDE.md 已经 800 行，AI 反而开始忽略其中一半指令，你问它"我的测试约定是什么"，它答得颠三倒四。文件从"帮手"变成了"噪音"。

先破一个误区：CLAUDE.md 不是越全越好。研究显示 LLM 可靠遵从大约 150 到 200 条指令，超过这个量，后面写的指令要么被忽略、要么与前面冲突。这是所有指令文件共同的天花板，[[01-AI技术/超长项目AI跑/02_上下文管理：AI的记忆体系|AI的记忆体系]] 讲过的"上下文稀释"在指令文件上一样成立。

## 四级层级

CLAUDE.md 不是单文件，而是从企业到本机的四级体系，按优先级从高到低：

1. 企业级：团队或组织共享的全局规则（由管理员维护）。
2. 用户级：`~/.claude/CLAUDE.md`，你的个人偏好——缩进习惯、常用工具链、对 AI 的通用要求。
3. 项目级：`./CLAUDE.md` 或 `./.claude/CLAUDE.md`，本项目的事实与约定。
4. 本地级：`CLAUDE.local.md`，不提交版本库的私有内容，比如个人本地路径、临时实验约定。

四级叠加时，靠近项目的层覆盖用户层。判断一条规则放哪层，只看一个标准：这条规则是"只对我个人成立"，还是"对所有人、所有项目成立"。前者放用户级，后者放项目级。

> [!tip] 层级就是作用域。团队约定绝不写进个人 CLAUDE.md，否则同事的 AI 看到的还是旧规则；本地路径只写进 .local 文件，别污染共享的版本库文件。

## 与 AGENTS.md 并存

Claude Code 同时读 CLAUDE.md 与仓库根的 AGENTS.md，两套并存时注意三点：

- 同一事项只写一处：两处写同一约定且不一致时，AI 随机遵从其中一个，调试无从下手。
- 普适事实进 AGENTS.md，工具专属进 CLAUDE.md：Claude Code 特有的东西（subagent 偏好、hooks 用法）只写进 CLAUDE.md。
- 文件可以互相引用：CLAUDE.md 里放一行"通用规则见 AGENTS.md"，避免重复维护。

## 200 行原则

`/init` 能自动生成 CLAUDE.md，但生成的是骨架，真正的功力在精简。经验上限是 200 行，理由有三：

- 模型对后置指令的遵从度随文本量衰减，指令越靠后越容易被"遗忘"。
- 超过 200 行后，你无法判断 AI 实际读了哪部分，调试无从下手。
- 指令之间存在隐含冲突，行数越多冲突概率越高。

精简不是删内容，是分拣。把 CLAUDE.md 里超过一半的内容搬去 `docs/` 目录，原文只留一行索引。CLAUDE.md 负责"每会话必读"，文档负责"按需深读"，这就是渐进式披露。

> [!tip] 200 行不是硬指标而是信号：当你的 CLAUDE.md 需要滚动两屏以上才能读完，说明它已经不是指令文件，而是文档堆。剪掉细节，留下索引。

## 用 @path 导入

CLAUDE.md 支持 `@路径` 语法导入其他文件：

```markdown
@docs/architecture.md
@docs/testing-conventions.md
```

被导入的文件在读取时拼接进上下文。这比复制内容好：内容只有一份，改动自动同步。适合放"需要 AI 知道但不必常驻"的资料，比如架构说明、API 手册。注意导入同样计入上下文预算，别把整个文档库都 `@` 进来。

## 按主题拆分 rules

大型项目里单篇 CLAUDE.md 装不下所有主题，用 `.claude/rules/*.md` 按主题拆分：

```
.claude/rules/
├── frontend.md      # 前端约定
├── backend.md       # 后端约定
├── testing.md       # 测试约定
└── git-workflow.md  # 提交与分支规范
```

拆分的收益是"按需加载"：做前端任务时读 frontend.md，不必把后端内容塞进每个会话。注意拆分是给 CLAUDE.md 减负，不是把 200 行换成五个 200 行文件——总量依然受同样的遵从上限约束。哪条规则"每次会话都要用"，哪条"相关任务才用"，写之前先分好类。

## Auto Memory 机制

Claude Code 会把每个项目的会话摘要自动存到 `~/.claude/projects/<项目>/memory/`，新会话自动参考。这是工具自带的记忆层，与 CLAUDE.md 互补：

- CLAUDE.md 是你主动维护的"长期规则"。
- Auto Memory 是工具自动沉淀的"会话事实"。

别把两者混用。临时结论（"今天决定先做导出功能"）交给 Auto Memory 去记，永久约定（"导出必须用 CSV 编码"）写进 CLAUDE.md。如果你发现 Auto Memory 记的内容比 CLAUDE.md 还准，说明 CLAUDE.md 该更新了。

memory/ 目录按项目隔离，随 `~/.claude` 一起备份，换电脑不会丢。想给某个项目单独加固记忆，可以在会话里明确要求，也可以定期把 Auto Memory 里的重要结论提炼进 CLAUDE.md——让工具的记忆倒灌回你的规则文件。

> [!tip] 你无法直接编辑 Auto Memory，但能通过会话总结间接影响它。每次会话结束时说一句"把 X 写进项目记忆"，比指望它自己猜更可靠。

## 与项目记忆文档的分工

[[05-产品与开发/AI驱动Web应用开发/02-文档体系篇/02_项目记忆文档|项目记忆文档]]（PROJECT_MEMORY.md）与 CLAUDE.md 是"事实"与"规则"的分工：

- CLAUDE.md 存规则：AI 该怎么做、不许怎么做、代码风格、工作流。
- PROJECT_MEMORY.md 存事实：项目是什么、技术栈是什么、当前状态到哪一步。

规则低频稳定、事实高频变化，混在一起必然互相拖累。判别口诀：能用"是"回答的进记忆文档，能用"应该、必须"回答的进 CLAUDE.md。"技术栈是 React"进记忆，"组件必须用 TypeScript"进规则。

## /init 与 /doctor

Claude Code 提供两个命令帮你管理 CLAUDE.md：

- `/init`：扫描仓库自动生成 CLAUDE.md 初稿，生成后必须人工校对，别直接提交。
- `/doctor`：检查 CLAUDE.md 并给出精简建议，定期跑一遍当作体检。

维护节奏：新项目用 `/init` 起步，每两周跑一次 `/doctor`，规则变更当天人工修订。CLAUDE.md 是活的文档，写了不维护等于没写。这套"规则文件 + 定期体检"的循环，与 [[01-AI技术/LOOP Engineering/03-架构篇/03_Hook Loop Harness范式|Hook Loop Harness]] 中"固定钩子定期触发"的思路同源。

## 模板

```markdown
# CLAUDE.md

## 项目
<一句话：这是什么项目，用什么技术栈>

## 常用命令
- 开发：`npm run dev`
- 测试：`npm test`
- 构建：`npm run build`

## 代码约定
- 组件用 TypeScript 函数式写法
- 测试放 tests/，命名 *.test.ts
- 提交信息遵循 Conventional Commits

## 工作流
- 每个需求先写测试再写实现
- 修改公共接口前先看影响范围

## 不要做
- 不要修改锁文件之外引入依赖
- 不要在未跑测试时提交

@docs/architecture.md
@docs/testing-conventions.md
```

模板里的"不要做"一节最容易被忽略，其实最值钱：模型对禁令的遵从度通常高于建议，把最容易犯的错写成禁令，比十条"应该"都有效。

## 现在你应该怎么做

1. 在项目里跑 `/init` 生成初稿，删掉模板废话，只留真实约束。
2. 把 CLAUDE.md 压到 200 行以内：规则留原文，细节改成 `@` 导入或索引。
3. 超过两个主题就建 `.claude/rules/` 按主题拆分，根文件只留跨主题规则。
4. 用一句口诀检查每条：能用"是"回答的，挪进 PROJECT_MEMORY.md。
5. 约定一个维护节奏（如每两周 `/doctor` 一次），让 CLAUDE.md 长期保持精瘦。
