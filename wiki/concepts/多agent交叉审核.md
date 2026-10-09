---
type: entity
title: 多 AI Agent 交叉审核
tags: [多agent, 交叉审核, 质量保证, codex, claude-code]
related: [smallnest-autoresearch, 奇偶轮角色互换, ai自审偏差, 多agent角色思维隔离, 高阶模型审查低阶模型, 双轨校验, 跨模型评估, 反馈驱动迭代]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# 多 AI Agent 交叉审核

多 AI Agent 交叉审核是 [[smallnest-autoresearch]] 的核心质量保证机制：让 Codex 与 Claude 轮流担任实现者与审核者（A 写完 B 审、B 写完 A 审），利用不同模型盲区与强项互补，发现单 Agent 自审发现不了的问题。它是本文对 Karpathy 单 Agent 自审路线的三大改进之首。

## 机制要点

- **角色互换调度**：奇数轮 Codex 开路、偶数轮 Claude 开路，消除固定先后偏差，详见 [[奇偶轮角色互换]]
- **组合弹性**：经近期优化支持 opencode 实现 1-3 个任意组合的 Coding Agent 交叉审核与代码实现
- **反馈闭环**：审核发现的不足直接注入下一轮提示词（见 [[反馈驱动迭代]]），使交叉审核不止于把关、更驱动改进
- **实现者也能被审出问题**：Issue #21 首轮 Codex 只读代码未实现（1.0 分）、由 Claude 审核发现不足；Issue #15 中 Claude 承担"补充实现"，说明角色并非固化为"实现者/审核者"单一身份

## 证据与边界

作者声明"实践证明，单 Agent 的效果远不如双 Agent 交叉审核"，但**无对照实验数据**支撑；三个案例日志为案例级自报证据（#21 有 asciinema 回放可部分验证），口径记录于 [[ai工程量化效果声明追踪]]。

## 跨团队印证

该机制与 wiki 既有概念形成方法族：[[ai自审偏差]]（单 Agent 自审不可靠的问题面）、[[多agent角色思维隔离]]（Specflow 的角色隔离）、[[高阶模型审查低阶模型]]（美团的 Judge Model）、[[双轨校验]] 与 [[跨模型评估]]（长程任务质量保证）。本文贡献的是**调度级对称互换**这一具体形态。
