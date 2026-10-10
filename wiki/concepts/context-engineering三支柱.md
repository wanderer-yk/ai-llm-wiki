---
type: concept
title: OpenClaw Context Engineering 三支柱
tags: [openclaw, context-engineering, 上下文工程]
related: [openclaw, context-window三段构成, compaction双触发模式, 自适应分块压缩, 工具结果头尾修剪, openclaw双层记忆系统, 记忆时间衰减, 上下文工程, 上下文压缩策略]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践.html"]
---

# OpenClaw Context Engineering 三支柱

OpenClaw Context Engineering 三支柱是 [[openclaw]] 应对上下文窗口爆炸的完整解析框架：①可扩展的 Agent Skills 机制；②动态的上下文压缩（Compaction）与修剪（Pruning）；③分层的记忆存储系统（Memory）。提出动机是上下文窗口爆炸与 Lost in the Middle 现象——一味堆砌 prompt/历史/工具结果导致推理耗时+成本飙升+注意力稀释，最终无法遵循核心指令。

作者总结三支柱的精髓为"图书管理员"类比：像图书管理员，懂得何时把书放进仓库（压缩/记忆），何时迅速抽出递给你（检索/注入）——在有限窗口内实现无限知识扩展、高效对话管理和持久记忆保持。

三支柱的工程落地分布：

- **Skills**：渐进式披露（先扫 available_skills 描述再按需读唯一 SKILL.md），ClawHub 类 App Store 扩展能力
- **Compaction & Pruning**：[[compaction双触发模式]] + [[自适应分块压缩]] + [[摘要分层降级策略]] + [[工具结果头尾修剪]] + [[kv-cache时间窗优化]]
- **Memory**：[[openclaw双层记忆系统]] + [[记忆时间衰减]]

该框架是通用 [[上下文工程]] 议题在 OpenClaw 上的系统实现，与 [[上下文压缩策略]]（"OpenClaw 记忆落盘+分阶段压缩+跨会话加载"）的既有描述形成代码级互证。

来源：[[sources/[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践|[202604130830] 深度解析 OpenClaw]]。
