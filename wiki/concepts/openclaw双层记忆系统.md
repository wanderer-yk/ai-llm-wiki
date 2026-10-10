---
type: concept
title: OpenClaw 双层记忆系统
tags: [openclaw, memory, 记忆管理]
related: [记忆时间衰减, markdown多层记忆体系, 记忆写入双路径, memory-flush机制, 混合召回, Evergreen免衰减, openclaw工作区md文件族, 记忆召回与反馈环]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践.html"]
---

# OpenClaw 双层记忆系统

OpenClaw 双层记忆系统是 [[openclaw]] 的分层记忆架构：长期记忆 `MEMORY.md`（高价值持久事实与偏好，每次对话自动注入 System Prompt，截断至 200 行，须精简、重要信息前置）+ 每日记忆 `memory/日期.md`（低频细节，防核心过载）。引擎代码 `src/memory/manager.ts`，工具代码 `src/agents/tools/memory-tool.ts`。

六维对比（原文）：

| 特性 | MEMORY.md（长期记忆） | memory/日期.md（每日笔记） |
|------|----------------------|---------------------------|
| 文件数量 | 只有一个 | 每天一个 |
| 写入方式 | 整理后写入（覆盖或编辑） | 追加写入（append） |
| 内容类型 | 持久的事实和偏好 | 每日的上下文笔记 |
| 注入方式 | 每次对话都注入到系统提示词 | 只通过搜索访问 |
| 时间衰减 | 不衰减（"保持常青"的内容） | 随时间衰减 |
| 适合记什么 | 比较重要的项目名称 | 今天讨论了API重构问题 |

- **写入双策略**：显式写入（"请记住…"）+ 隐式闪存（Memory Flush：会话结束/新 Session/触发压缩时自动提炼归档）——与 [[记忆写入双路径]]、[[memory-flush机制]] 直接互证
- **召回三级入口**：被动注入（BM25+向量双路，即 [[混合召回]]）→ `memory search` 主动搜索 → 按行号深层钻取原始文件；索引为"切片+向量化+SQLite 存储"
- **遗忘机制**：无自动删除、需人工清理；MEMORY.md 永不衰减（"保持常青"，与 [[Evergreen免衰减]] 字面级互证）；每日笔记按 [[记忆时间衰减]] 降低检索权重
- **安全隔离**：MEMORY.md 仅主会话加载，群聊模式不加载以防个人上下文泄露

本文与 [[sources/[202604151800]OpenClaw长期记忆优秀管线与玄学效果|[202604151800] OpenClaw 长期记忆优秀管线与玄学效果]] 描述同一体系，全要素互证；对照页面另有 [[markdown多层记忆体系]] 与 [[记忆召回与反馈环]]。

来源：[[sources/[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践|[202604130830] 深度解析 OpenClaw]]。
