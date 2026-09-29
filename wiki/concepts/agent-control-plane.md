---
type: concept
title: Agent Control Plane
tags: [agent, 治理, 控制平面, 权限, 安全]
related: [zhiyuanfu, 24h打工人, harness-engineering, 脚手架优于模型, agent可观测性六维度]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202605071734]十年老技术开发的AIAgent探索之路.html"]
---
# Agent Control Plane

Agent 系统控制平面，是从 demo 到系统的关键跨越。"从 demo 到系统，中间隔着的不是更多 Prompt，而是 control plane。"

## 核心组成

- **工具调用权限** — Agent 可调用哪些工具
- **人工确认阈值** — 哪些操作需人工审批
- **读写路径隔离** — Agent 可读写哪些目录/文件
- **失败熔断规则** — 连续失败时的自动保护机制

## Agent 可观测性六维度

| 维度 | 含义 |
|------|------|
| goal | 当前目标是什么 |
| step | 正在执行哪个步骤 |
| tool | 使用了什么工具 |
| failure | 为什么失败 |
| recovery | 如何恢复 |
| cost | 花了多少成本 |

## 与其他治理框架的关联

- 与 [[harness-engineering]] 的五要素工程化约束高度共鸣——都强调在 Agent 执行前建立完整的约束边界
- "先投治理再投模型"与 [[高阶模型审查低阶模型]] 方向一致但视角不同