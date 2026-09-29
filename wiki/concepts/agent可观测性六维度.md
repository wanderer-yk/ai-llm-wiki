---
type: concept
title: Agent 可观测性六维度
tags: [agent, 可观测性, 治理, 评估]
related: [zhiyuanfu, agent-control-plane, sdd留痕进化论, agent评测系统]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202605071734]十年老技术开发的AIAgent探索之路.html"]
---
# Agent 可观测性六维度

[[zhiyuanfu]] 提出的生产级 Agent 系统最低可观测性要求。六个维度覆盖 Agent 运行的完整生命周期。

| 维度 | 含义 |
|------|------|
| **goal** | 当前目标是什么 |
| **step** | 正在执行哪个步骤 |
| **tool** | 使用了什么工具 |
| **failure** | 为什么失败 |
| **recovery** | 如何恢复 |
| **cost** | 花了多少成本 |

## 与评估的关系

评估不是验收动作而是日常运行信号。通过可观测性六维度持续回答：失败切换有没有制造新错误？需求澄清是否稳定？任务拆解是否越来越合理？Skill 是否真提高成功率？

## 与已有概念关联

- 是 [[sdd留痕进化论]] 从 debug 到 optimize 的闭环升级
- 与美团 [[agent评测系统]] 的评测思路呼应——都强调评估作为运行信号而非验收