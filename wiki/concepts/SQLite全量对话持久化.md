---
type: concept
title: SQLite全量对话持久化
tags: [记忆系统, 数据资产, hermes-agent, sqlite]
related: [hermes-agent, openclaw, 内外双路径自进化, agent轨迹, 内外双驱记忆架构, 知识复利效应, 长记忆四件套, file-as-progress状态持久化]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践.html"]
---
# SQLite全量对话持久化

SQLite全量对话持久化是一种对话存储策略：将 Agent 的完整对话历史以结构化形式存入 SQLite 数据库，使历史对话成为可查询、可索引、可再利用的数据资产，而非仅服务记忆召回的临时日志。来源文章将其归于 [[hermes-agent]]，并与 [[openclaw]] 的持久化口径做了关键区分。

## 与 OpenClaw 的存储对象差异

两者同样采用 SQLite 做持久化，但存储对象不同：

- **Hermes**：存全部每日对话历史（全文）
- **OpenClaw**：存 Memory Chunk 索引（非对话全文），服务记忆召回

## 双重目的

1. **结构化数据资产**：对话历史可查询、可索引、可按主题检索、可按时间回溯
2. **赋能自进化闭环**：高质量轨迹是生成 Skill 和 RL 训练的最原始素材（见 [[内外双路径自进化]]）；非结构化日志在大规模提取、清洗、格式化时低效易错，数据库化便于高效处理

存储的对话历史即 [[agent轨迹]] 的原始来源，经 `agent/trajectory.py` 转换为 ShareGPT 格式后进入训练流水线。

## 口径演化说明

来源文章内部对此有一段逐步精确化的表述过程：先称 OpenClaw 执行过程"无状态"（对照 Hermes 的经验沉淀），后承认 OpenClaw 亦有 SQLite 持久化，最终精确化为"OpenClaw 存 Memory Chunk 索引、Hermes 存全部对话历史"——两者是持久化**对象**之别而非**有无**之别。引用时须注意这一口径。

## 与其他持久化方案的对照

- [[openclaw]]：SQLite 存 Memory Chunk 索引，服务记忆召回
- [[长记忆四件套]]（小红书 PMO）：SessionMessage 等自建数据模型
- [[file-as-progress状态持久化]]：以文件为进度载体；SQLite 为结构化数据库载体
- 数据资产化视角与 [[知识复利效应]] 呼应：沉淀的历史数据随规模增长加速释放价值
