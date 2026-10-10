---
type: concept
title: skill 轻量索引按需加载
tags: [hermes-agent, skill, 上下文工程]
related: [hermes-agent, openclaw, 渐进式披露替代向量检索, nested_memory按需加载, skill自动创建触发条件]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604230830]深入源码HermesAgent如何实现SelfImproving.html"]
---
# skill 轻量索引按需加载

skill 轻量索引按需加载是 [[hermes-agent]] 的 Skill 上下文加载策略：系统提示词只放**轻量索引**（每个 Skill 的名字 + 一句话描述），任务相关时才通过 `skill_view` 加载全文——作者称之为"动态图书馆"模式。收益是省 Token、避免无关 Skill 稀释模型注意力。

## 与 OpenClaw 的对比（作者单方定性）

文章将 [[openclaw]] 的做法（SOUL.md/IDENTITY.md 等全量塞入上下文）比喻为**"重型背包"**模式，认为其导致 Token 浪费与注意力稀释。此对比为作者观点，引用需归因。

## 同族方案交叉对照

本 Wiki 中同属"按需加载/渐进披露"谱系的方案：

- [[渐进式披露替代向量检索]]（有赞共享技术）：目录结构即检索路径，YAML description 做摘要判断；
- [[nested_memory按需加载]]（Claude Code 源码分析）：目录级 memory 文件按需加载。

Hermes 的差异点在于索引常驻系统提示词、全文按需 `skill_view`，且加载动作由 Agent 在任务执行中自主触发。