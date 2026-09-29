---
type: entity
title: yabohe
tags: [人物, 腾讯, ai-agent, 框架设计]
related: [腾讯技术工程, codebuddy, react-agent, 上下文工程]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604131736]详尽地带你从零开始设计实现一个AIAgent框架.html"]
---
# yabohe

腾讯技术工程团队成员，署名"腾讯程序员"。2026 年 4 月在 [[腾讯技术工程]] 微信公众号发表《详尽地带你从零开始设计实现一个AI Agent框架》一文，系统梳理了 AI Agent 框架的理论基础与实践实现。

## 核心贡献

- 完成三大基础 Agent 模式（[[react-agent|ReAct]] / [[plan-and-execute模式]] / [[reflection模式]]）的学术溯源
- 提出 [[agent框架三要素]]（LLM Call + Tools Call + Context Engineering）解构框架
- 实现 279 行 Python 单文件的极简 [[agent-loop|Agent Loop]] 框架
- 提出 [[agent三层商用架构]]（框架 + 上下文工程 + Skills）
- 揭示 [[manus|Manus]] 不使用 MCP 而基于 [[codeact架构|CodeAct]] 的技术路线
- 引用 Shunyu Yao 团队量化数据论证 [[上下文工程]] 的关键价值

## 与其他实体的关系

- 与 [[binxiong]] 同属 [[腾讯技术工程]] 公众号作者，binxiong 聚焦 [[ai-native研发模式]]，yabohe 聚焦 Agent 框架底层设计
- 文中提及 [[codebuddy]] Agent SDK 及其衍生的 WorkBuddy 应用