---
type: concept
title: AI 编程四阶段演进
tags: [ai编程, 范式演进, ralph-loop, vibe-coding]
related: [ralph-loop, vibe-coding, 任务隔离, human-in-the-loop, task-driven对goal-driven, agent-loop]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601270830]从ReAct到RalphLoopAIAgent的持续迭代范式.html"]
---

# AI 编程四阶段演进

AI 编程四阶段演进是来源文章（技巧 1"理解 Ralph 是一个循环"）给出的范式演进框架：每一阶段解决前一阶段的局限，终点是 Ralph Loop。

| 阶段 | 做法 | 局限 |
|---|---|---|
| 1. Vibe 编程（[[vibe-coding]]） | 让 AI 写代码而不真正检查 | 速度快，但代码质量差 |
| 2. 规划模式 | 先规划后编码（如 Claude Code 规划模式：AI 探索代码库并创建计划） | 受限于单个上下文窗口 |
| 3. 多阶段计划 | 每个阶段在单独的上下文窗口中处理（[[任务隔离]]） | 需人工持续编写提示衔接 |
| 4. Ralph Loop（[[ralph-loop]]） | 同一提示循环运行，代理自主从 PRD 选择任务 | 需可验证完成条件与成本控制 |

## 关键改进

从阶段 3 到阶段 4 的本质跃迁："代理选择任务，而不是你。你只定义最终状态"——这正是 [[task-driven对goal-driven]] 跃迁框架在本来源中的直接表述。

## 运行模式配套

阶段 4 配套 [[hitl到afk渐进路径]]：先用 HITL（结对编程式）学习与优化提示，提示稳定后再转 AFK 无人值守。