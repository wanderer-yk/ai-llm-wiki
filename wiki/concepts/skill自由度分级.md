---
type: concept
title: Skill 自由度分级
created: 2026-10-13
updated: 2026-10-13
tags: [skill, 最佳实践, 设计决策]
related: [skill简洁性原则, skill工作流与反馈循环模式, skill-creator]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Skill 自由度分级

Skill 自由度分级是 Anthropic 官方最佳实践（经 writing-skills 的 anthropic-best-practices.md 转述）中的 Skill 设计决策框架：写 Skill 前先判断该任务允许多少执行自由度，再决定指令的具体程度。

## 三级决策

| 自由度 | 适用情形 | 指令策略 |
|---|---|---|
| 高 | 多种方法都能达到目标 | 只给目标与约束，允许 Agent 自主发挥 |
| 中 | 存在首选模式但允许变化 | 给出首选路径并说明可接受的偏差 |
| 低 | 操作脆弱、一致性关键（如部署步骤） | 逐步骤严格脚本化，禁止偏差 |

## 意义

该分级与 [[skill简洁性原则]]（假设模型已聪明、只添加增量信息）和 [[skill工作流与反馈循环模式]]（一致性关键任务的检查清单/验证器结构）共同构成 Anthropic 官方 Skill 写作三支柱。它与 Google 侧 [[skill五种设计模式]] 中的 Pipeline（低自由度模式化）和 [[skill-creator]] 的"解释为什么而非堆砌必须"（高自由度模式化）互为镜像。
