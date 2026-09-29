---
type: concept
title: Agent 框架三要素
tags: [ai-agent, 框架设计, 上下文工程, 工程变量分析]
related: [agent-loop, 上下文工程, react-agent, codeact架构]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604131736]详尽地带你从零开始设计实现一个AIAgent框架.html"]
---
# Agent 框架三要素

由 [[sources/[202604131736]详尽地带你从零开始设计实现一个AIAgent框架|yabohe（腾讯技术工程）]]提出的 Agent 框架解构模型。任何 Agent 框架都可拆解为三个核心要素：

## 三要素定义

| 要素 | 本质 | 工程变量 |
|------|------|----------|
| **LLM Call** | 推理本质 | 变量最小——LiteLLM 等工具已成熟 |
| **Tools Call** | 执行本质（代码是 Tools 的一种） | 场景依赖，有最佳实践 |
| **Context Engineering** | 连接推理与执行的核心 | **最大变量，智能核心** |

## 工程变量分析

- **LLM Call**：OpenAI SDK 兼容接口已成行业标准，LiteLLM 可屏蔽不同 LLM API 差异，无需重复造轮子
- **Tools Call**：演进脉络为 Function Call → MCP → Skills；主流工具类型为文件操作、网络搜索、Shell/代码执行、API/MCP 调用
- **Context Engineering**：是"低垂的果实"，仍有很大优化空间

## 上下文工程的狭义与广义

- **狭义**：Prompt 工程（Rules / Claude.md / AGENTS.md）
- **广义**：工具与提示词结合（如 Skills）

## 核心洞察

> 代码库本身也是上下文工程的一部分。代码库越简单，上下文越清晰，Agent 越智能。

这一洞察将代码质量直接关联到 Agent 智能程度，为 [[极简agent设计哲学]] 提供了理论基础。

## 量化证据

Shunyu Yao（姚顺宇，ReAct 论文作者）团队研究：GPT-5.1 (High) 在不提供任何 Context 的情况下仅能解决不到 1% 的任务，有力证明了 [[上下文工程]] 的关键价值。