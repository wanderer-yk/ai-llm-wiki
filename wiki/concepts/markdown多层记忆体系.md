---
type: concept
title: Markdown 多层记忆体系
tags: [openclaw, 长期记忆, markdown]
related: [openclaw, LLM弱约束记忆决策, 记忆写入双路径, 长记忆四件套, memdir结构化记忆系统]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604151800]OpenClaw长期记忆优秀管线与玄学效果.html"]
---
# Markdown 多层记忆体系

Markdown 多层记忆体系是 [[openclaw]] 记忆系统的核心设计原则："**一切持久状态都是磁盘上的 Markdown 文件**"，会话启动时按优先级注入系统提示词。与文件系统作为上下文的行业共识一致，但本文提供了迄今最细粒度的文件级披露。

## 8 文件体系（原文表格）

| 文件 | 用途 | 加载时机 |
|------|------|----------|
| `AGENTS.md` | 工作区规则、安全边界、红线指令 | 每次会话（最高优先级） |
| `SOUL.md` | Agent 个性、价值观、沟通风格 | 每次会话 |
| `IDENTITY.md` | Agent 身份元数据（名字、角色、头像） | 每次会话 |
| `USER.md` | 用户档案（名字、昵称、时区、个人背景） | 每次会话 |
| `TOOLS.md` | 环境配置（设备信息、SSH 主机、TTS 偏好） | 每次会话 |
| `MEMORY.md` | 长期记忆（已验证事实、决策、持久学习） | 仅 DM 主会话 |
| `memory/YYYY-MM-DD.md` | 日记忆（当天观察、临时笔记） | 当天 + 昨天自动加载 |
| `DREAMS.md` | 梦境日记（Dreaming 系统输出，仅供人类审查） | 不自动注入 |

## 两套机制

身份/规则/用户档案类文件（AGENTS/SOUL/USER.md）与动态记忆类文件（MEMORY.md、memory/YYYY-MM-DD.md）是**两套不同机制**：前者是档案式维护（USER.md 模板明文 "Update this as you go"），后者有专门的写入、演进、召回管线。

## 设计哲学引文（AGENTS.md 默认模板）

```
"Memory is limited — if you want to remember something, WRITE IT TO A FILE.
'Mental notes' don't survive session restarts. Files do."
```

## 跨来源对照

- 文件承载记忆 ↔ [[长记忆四件套]]（小红书 PMO，UserProfile/SessionMessage/KnowledgeItem/ContextBuilder 结构化记忆）、[[memdir结构化记忆系统]]（Hermes）：Markdown 自由文本 vs 结构化存储的路线对照。
- 多层文件分层 ↔ [[多源分治策略]]（爱奇艺）：按职责分配到稳定位置的同构思想。
