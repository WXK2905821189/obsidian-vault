---
title: "三、主流 AI Agent 产品介绍"
source: "https://agent.deepseektavern.com/"
created: 2026-07-02
updated: 2026-07-02
chapter: 3
cssclasses:
  - ai-agent-guide
tags:
  - AI-Agent
  - 入门指南
  - DeepSeekTavern
  - Obsidian
---

# 三、主流 AI Agent 产品介绍

> [!info]+ 文档信息
> - 来源：[agent.deepseektavern.com](https://agent.deepseektavern.com/)
> - 整理日期：2026-07-02
> - 所属知识库：[[00-目录|AI Agent 完全入门指南]]

> [!abstract]+ 本章速览
> - [[#3.1 小龙虾（OpenClaw / QClaw）|3.1 小龙虾（OpenClaw / QClaw）]]
> - [[#3.2 Hermes Agent|3.2 Hermes Agent]]
> - [[#3.3 Claude Code|3.3 Claude Code]]
> - [[#3.4 OpenAI Codex|3.4 OpenAI Codex]]
> - [[#3.5 OpenCode|3.5 OpenCode]]
> - [[#3.6 WorkBuddy|3.6 WorkBuddy]]
> - [[#3.7 六款产品横向对比|3.7 六款产品横向对比]]

> [!tip] 阅读导航
> [[00-目录|目录]] · [[02-AI Agent 的分类|上一篇]] · [[04-AI Agent 能做什么|下一篇]]

---

## 3.1 小龙虾（OpenClaw / QClaw）

"小龙虾"是中国用户对 **OpenClaw** 的昵称，此外还有腾讯推出的 QClaw（本地虾）等同类产品。它是 2026 年最火的开源 **AI Agent** 框架之一。

基本信息：由奥地利程序员 Peter Steinberger（PSPDFKit 创始人）开发，使用 TypeScript 编写，MIT 协议完全开源。2026年1月正式定名 **OpenClaw**，曾用名 ClawdBot、Moltbot。

核心定位："数字执行官"——本地优先、可自主执行任务的 AI 智能体框架。核心是把自然语言指令转化为电脑实际操作，实现"一句话让 AI 替你干活"。

核心特点：本地部署，隐私可控，数据不离开本地；拥有 Shell 级访问权，可直接操控文件、终端、浏览器、鼠标键盘；支持自主任务闭环（下达目标、自动拆解、执行、纠错、完成）；兼容多种技能（Skills）扩展；腾讯云、小米等国内大厂纷纷推出部署方案。

适合人群：有一定技术基础的用户、注重隐私的用户、希望深度定制 AI 工作流的高级用户。

## 3.2 Hermes Agent

**Hermes Agent** 是由知名 AI 研究实验室 Nous Research 于 2026 年 2 月发布的开源自主 AI 智能体，上线仅六周就突破 4.7 万 GitHub 星标。

核心定位："唯一能自我进化的 Agent"——它在使用中会自主创建技能、改进技能、把重要事实写入持久化记忆、检索历史会话，并建立对用户的精准画像。

核心特点：内置自学习闭环，能从任务经验中自动生成可复用技能文件；基于 Honcho 协议构建记忆系统，支持跨会话记忆搜索；支持 Telegram、Discord、Slack、微信等 12 个以上平台接入；兼容 200 多种主流 **大模型**（千问、**GLM**、**Kimi**、OpenAI 等）；40 多种内置工具 + **MCP** 集成；支持 6 种运行环境（本地、Docker、SSH、无服务器等）。

适合人群：开发者、技术人员、多平台用户、希望 AI "越用越聪明"的长期使用者。

## 3.3 Claude Code

**Claude Code** 是 Anthropic 公司推出的官方 AI 编程助手，定位为"开箱即用的完整产品"。

核心定位：功能全面、开箱即用的"智能成品"，由 Anthropic 官方打造。主要面向编程和软件开发场景。

核心特点："内置一切"，提供 18 种以上工具、子代理、权限系统、LSP 等；主要依赖 Claude 系列模型，代码理解能力顶尖；适合大型项目的复杂重构；订阅制，约 20 美元/月；官方支持，生态完善，集成度高。

注意事项：**Claude Code** 对 IP 审查较为严格，存在封号风险；重度使用可能产生额外计费；主要面向开发者，非技术人员上手有一定门槛。

适合人群：专业开发者、需要进行大型代码重构的程序员、希望开箱即用的用户。

## 3.4 OpenAI Codex

**OpenAI Codex** 是 OpenAI 推出的编码 Agent，2026 年进行了重大升级，从简单的代码补全工具进化为完整的 AI 编程代理。

核心定位："从问答转向执行"的 AI 编码代理。2026年4月进一步扩展为桌面级 **AI Agent**，能操作整个电脑。

核心特点：深度集成在 ChatGPT 生态中，支持自然语言编程；云端沙箱环境，可读写 GitHub 仓库、创建 PR；支持 90 多种插件（Jira、GitLab、Microsoft Suite 等）；具备背景电脑使用能力（在 macOS 上可以看到、点击、输入）；持久记忆与任务调度；内置 GPT-5.5+ 语言模型和 GPT Image 2 图像模型。

适合人群：ChatGPT 重度用户、需要云端编码环境的开发者、希望 AI 操作整个桌面的用户。

## 3.5 OpenCode

OpenCode 是一款开源的 AI 编码代理，提供终端界面、桌面应用和 IDE 扩展等多种使用方式。

核心定位：开源、可定制的编程 Agent，强调灵活性和可扩展性。

核心特点：完全开源，可自由定制和修改；提供主 Agent 和子 Agent 的双层架构（Build/Plan 主 Agent + General/Explore/Scout 子 Agent）；支持在会话中切换不同 Agent；可通过 @ 提及调用专门 Agent；支持自定义提示词、模型和工具访问权限；适配多种开发场景。

适合人群：喜欢自己动手的开发者、需要灵活定制 Agent 行为的用户、终端爱好者。

## 3.6 WorkBuddy

**WorkBuddy** 是腾讯云 CodeBuddy 团队推出的 **AI Agent** 办公工具，被称为"腾讯版小龙虾"。

核心定位：全场景职场 AI 智能体桌面工作台——面向非技术背景的职场人群，零门槛实现 AI 办公提效。

核心特点：一句话指令即可自主规划并交付完整结果（文档、表格、PPT、数据分析报告等）；多 Agent 并行协作，一个人顶一支团队；支持 **MCP** 生态 + 自定义 Skills，能力无限扩展；内置混元、**DeepSeek**、**GLM**、**Kimi** 等多款模型可切换；兼容 **OpenClaw** 社区技能，无缝接入企业微信、QQ、飞书、钉钉；企业级安全沙箱，文件夹级授权 + 高危操作拦截；已在腾讯内部超过 2000 名员工深度使用。

与 **OpenClaw** 的关系：**WorkBuddy** 常被称作"腾讯版小龙虾"。两者互补——**OpenClaw** 是面向技术人的"数字员工操作系统"，追求极限灵活性；**WorkBuddy** 是面向普通人的"开箱即用智能同事"，追求极致易用性。

适合人群：职场白领、非技术背景用户、企业团队、需要办公自动化的重度办公人群。

## 3.7 六款产品横向对比

| 产品 | 开发者 | 开源 | 定位 | 上手难度 | 适合人群 |
| --- | --- | --- | --- | --- | --- |
| 小龙虾 | Peter Steinberger | 是（MIT） | 本地执行框架 | 较高 | 技术用户 |
| Hermes | Nous Research | 是（MIT） | 自进化智能体 | 中等 | 开发者/多平台 |
| Claude Code | Anthropic | 否 | 编程助手 | 低 | 专业开发者 |
| Codex | OpenAI | 否 | 云端编码代理 | 低 | ChatGPT 用户 |
| OpenCode | 社区 | 是 | 可定制编码代理 | 中等 | 终端爱好者 |
| WorkBuddy | 腾讯云 | 否 | 职场办公工作台 | 很低 | 职场办公用户 |

---

> [!tip] 阅读导航
> [[00-目录|目录]] · [[02-AI Agent 的分类|上一篇]] · [[04-AI Agent 能做什么|下一篇]]
