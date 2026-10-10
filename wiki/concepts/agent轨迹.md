---
type: concept
title: Agent 轨迹
tags: [trajectory, 数据格式, hermes-agent, 自进化]
related: [hermes-agent, sharegpt格式, 批量数据生成, 动态skill生成, 后台审查agent, research-ready训练闭环, 内外双路径自进化, SQLite全量对话持久化]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践.html"]
---
# Agent 轨迹

Agent 轨迹（Trajectory）在来源文章中被定义为**完整任务对话记录**：包括系统提示词、用户请求、思考行动与工具调用结果。它是 [[hermes-agent]] 自进化双路径的**共同原料**——既是 [[动态skill生成]] 与 [[后台审查agent]] 的复盘对象，也是 RL 训练的数据来源（见 [[内外双路径自进化]]）。

## 组织与预处理（`agent/trajectory.py`）

核心函数：`save_trajectory`（保存轨迹）、`convert_scratchpad_to_think`（将 `<REASONING_SCRATCHPAD>` 转换为 `<think>` 标签）、`has_incomplete_scratchpad`（检测未闭合的推理段）。轨迹统一导出为 [[sharegpt格式]]。

## JSONL 记录结构

每条记录含四字段：`conversations`（ShareGPT 格式对话）、`timestamp`、`model`、`completed`。成功与失败轨迹分文件输出：`trajectory_samples.jsonl` 与 `failed_trajectories.jsonl`（失败轨迹在后续训练中的具体用途来源未说明，属开放问题）。

## 与持久化的关系

运行时全部每日对话历史经 [[SQLite全量对话持久化]] 落库，使轨迹成为可查询、可索引、可按主题/时间提取的结构化资产，为大规模提取/清洗/格式化提供基础——这是轨迹得以作为“最原始素材”供给 Skill 生成与 RL 训练的数据工程前提。

原始轨迹可达几十万 Token（一次复杂对话调用十几次工具），直接用于 RL 训练不现实，须经 [[轨迹头尾保护压缩]] 压缩。