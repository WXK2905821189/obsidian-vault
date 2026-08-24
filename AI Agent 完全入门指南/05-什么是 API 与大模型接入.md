---
title: "五、什么是 API 与大模型接入"
source: "https://agent.deepseektavern.com/"
created: 2026-07-02
updated: 2026-07-02
chapter: 5
cssclasses:
  - ai-agent-guide
tags:
  - AI-Agent
  - 入门指南
  - DeepSeekTavern
  - Obsidian
---

# 五、什么是 API 与大模型接入

> [!info]+ 文档信息
> - 来源：[agent.deepseektavern.com](https://agent.deepseektavern.com/)
> - 整理日期：2026-07-02
> - 所属知识库：[[00-目录|AI Agent 完全入门指南]]

> [!abstract]+ 本章速览
> - [[#5.1 什么是 API|5.1 什么是 API]]
> - [[#5.2 如何接入大模型|5.2 如何接入大模型]]
> - [[#5.3 Base URL 格式汇总|5.3 Base URL 格式汇总]]

> [!tip] 阅读导航
> [[00-目录|目录]] · [[04-AI Agent 能做什么|上一篇]] · [[06-大模型核心参数详解|下一篇]]

---

## 5.1 什么是 API

**API**（Application Programming Interface，应用程序编程接口）是不同软件系统之间"对话"的标准化方式。你可以把它想象成餐厅的服务员——你不需要亲自进厨房做饭，只需要告诉服务员你想吃什么（发送请求），服务员就会把菜端给你（返回结果）。

对于**大模型**来说，**API** 就是你和 AI "对话"的通道。你的应用通过 **API** 把文字、图片等数据发送给 **大模型**服务商（如 OpenAI、阿里云、**DeepSeek**），服务商的模型处理完后，再通过 **API** 把结果返回给你。全程不需要你关心模型是怎么训练的、服务器在哪里——你只需要按照 **API** 的格式发送请求即可。

使用 **API** 的三大优势：

第一，即开即用。不需要自己购买 GPU、部署模型，注册账号拿到 **API** Key 就能开始调用，几分钟内就能让应用拥有 AI 能力。

第二，弹性扩展。**API** 采用按量付费模式，用多少付多少。今天调用 100 次，明天调用 10 万次，服务商的服务器会自动弹性伸缩，你不需要关心底层基础设施。

第三，持续迭代。模型会不断升级（比如 GPT-4 到 **GPT-4.5**），你只需在 **API** 请求中修改模型名称参数，就能无缝切换到最新版本，无需改动任何业务逻辑。

## 5.2 如何接入大模型

接入**大模型**的标准流程非常简单，核心就三步：

第一步：注册并获取 **API** Key。前往模型服务商的官网（如 OpenAI、阿里云百炼、**DeepSeek**、火山引擎等），注册账号后创建一个 **API** Key。这个 Key 相当于你的"密码"，用于验证你的调用权限。务必妥善保管，不要泄露到公开代码中。

第二步：配置 **Base URL** 和模型名称。在你的代码中设置两个核心参数：**Base URL**（**API** 的服务地址，不同服务商地址不同）和 model（要调用的具体模型名称，如 gpt-4o、deepseek-v3 等）。

第三步：发送请求并处理响应。使用 HTTP 客户端（如 Python 的 requests 库或 OpenAI SDK）按照 **API** 格式发送请求，获取模型的回复结果。几乎所有主流服务商都提供了 Python SDK，让接入更加简单。

以下是使用 Python 接入**大模型**的最小示例：

from openai import OpenAI
client = OpenAI(api\_key="sk-xxxx", base\_url="https://api.openai.com/v1")
resp = client.chat.completions.create(
model="gpt-4o",
messages=[{"role": "user", "content": "你好"}])
print(resp.choices[0].message.content)

接入国内大模型的流程完全相同，只需替换 Base URL 和 API Key 即可。例如接入 DeepSeek：将 base\_url 改为 https://api.deepseek.com，model 改为 deepseek-chat，即可调用 DeepSeek 的能力。

## 5.3 Base URL 格式汇总

不同的 **API** 协议有不同的 **Base URL** 格式。以下是 2026 年主流 **大模型** **API** 的格式汇总：

| API 协议 | Base URL 格式 | 代表厂商 |
| --- | --- | --- |
| OpenAI Chat Completions | https://api.xxx.com/v1 | OpenAI、DeepSeek、豆包、智谱、Kimi |
| OpenAI Responses API | https://api.xxx.com/v1 | OpenAI、阿里云百炼 |
| Anthropic Messages | https://api.xxx.com/v1/messages | Anthropic、DeepSeek（兼容） |
| Google Gemini | https://generativelanguage.googleapis.com | Google |
| 阿里云百炼 | https://dashscope.aliyuncs.com/compatible-mode/v1 | 通义千问、百炼平台 |
| 火山引擎方舟 | https://ark.cn-beijing.volces.com/api/v3 | 豆包、火山方舟 |
| OpenRouter（中转） | https://openrouter.ai/api/v1 | 100+ 模型统一接口 |

补充说明：

OpenAI Chat Completions 是行业最通用的标准格式，绝大多数国内厂商（**DeepSeek**、**豆包**、智谱、**通义**等）都兼容此格式。这意味着你只需修改 base\_url 和 model 参数，就能在不同模型之间无缝切换。

OpenAI Responses **API** 是 2025 年底推出的新一代接口，支持多轮对话自动关联（previous\_response\_id）、深度思考（reasoning）、内置 **工具调用**等高级功能。目前仅 OpenAI 官方和阿里云百炼等少数平台支持。

Anthropic Messages **API** 是 Claude 系列模型的原生格式，特点是 system 提示词作为独立参数（不在 messages 数组中），响应结构也不同。部分国内平台（如 **DeepSeek**）已提供兼容端点。

使用中转平台（如 OpenRouter）可以用统一接口访问 100+ 模型，非常适合需要快速对比不同模型效果的场景。

---

> [!tip] 阅读导航
> [[00-目录|目录]] · [[04-AI Agent 能做什么|上一篇]] · [[06-大模型核心参数详解|下一篇]]
