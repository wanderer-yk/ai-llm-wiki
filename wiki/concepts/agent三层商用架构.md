---
type: concept
title: Agent 三层商用架构
tags: [ai-agent, 商用架构, skills, 上下文工程]
related: [agent框架三要素, agent-loop, 极简agent设计哲学, 生产级skill, 三大武器库]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604131736]详尽地带你从零开始设计实现一个AIAgent框架.html"]
---
# Agent 三层商用架构

由 [[sources/[202604131736]详尽地带你从零开始设计实现一个AIAgent框架|yabohe（腾讯技术工程）]]在文章结尾提出的 Agent 商用化架构模型：

## 三层定义

| 层级 | 职责 | 内容 |
|------|------|------|
| **框架层** | 提供基础工具 | 极简工具集（如 shell_exec / file_read / file_write / python_exec） |
| **上下文工程层** | 提供环境 | 短期/长期记忆、主动/被动记忆、Session 管理、动态 RAG |
| **Skills 层** | 提供商业领域知识 | 特定业务场景的 SOP 封装与领域专业知识 |

## 核心洞察

框架层的极简设计与上下文工程层、Skills 层的丰富性形成互补。工业级 Agent 产品（如 [[openclaw|OpenClaw]] 的 [[pi-agent|Pi Agent]]）验证了这一架构——底层仅 4 个核心工具，但通过事件机制和 Skills 扩展实现丰富的业务能力。

## 与其他概念的关系

- Skills 层与腾讯 [[三大武器库]] 中的 Skills 概念、[[生产级skill]] 的理念一致
- 上下文工程层与 [[上下文工程]]（马上消费）指向同一核心理念
- 框架层的极简哲学即 [[极简agent设计哲学]] 的工程体现