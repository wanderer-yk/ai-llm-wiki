---
type: concept
title: 双计数器 nudge 触发
tags: [hermes-agent, nudge-engine, self-improving]
related: [hermes-agent, 后台审查agent, memory-skill-nudge三子系统自进化闭环, 全生命周期hook机制, skill自动创建触发条件]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604230830]深入源码HermesAgent如何实现SelfImproving.html"]
---
# 双计数器 nudge 触发

双计数器 nudge 触发是 [[hermes-agent]] Nudge Engine 的核心触发机制：维护两个**独立的经验计数器**，分别统计距上次主动反思的进度，各自达到阈值（默认均为 10）后触发后台审查：

- **Memory 计数器按"用户回合"计**（`run_agent.py:1328-1331`）——因为 Memory 的信息来源是用户输入；
- **Skill 计数器按"迭代"计**（`run_agent.py:1428-1431`）——因为经验源于工具使用过程。

Agent 主动调用 `memory`/`skill_manage` 时，对应计数器重置（刚反思过就不再催）。

## 频率权衡（文章设计取舍表第 5 条）

| 设计决策 | 表面效果 | 背后的考量 |
|---------|---------|-----------|
| Nudge 计数器可配置 | 默认 10 | 太频繁浪费 API 成本，太稀疏错过学习机会 |

## 触发后的动作

触发后在**用户响应发送给用户之后**，于后台 fork 独立 review agent 完成经验沉淀（详见 [[后台审查agent]]），保证"自省不应占用用户任务的 attention budget"。

## 关联

与 0424 飞樰文的 [[全生命周期hook机制]] 互补：hook 机制回答"在哪些生命周期节点干预"，双计数器回答"以什么节奏触发反思"。