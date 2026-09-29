---
type: concept
title: F-Harness
tags: [harness-engineering, multi-agent, anthropic, 验证机制]
related: [harness-engineering-李伟山版, AI自恋问题, anthropic, 可靠性边界论, 多agent角色思维隔离, 高阶模型审查低阶模型]
created: 2026-07-20
updated: 2026-07-20
sources: ["[202605201731]干货从PromptContext到Harness工程的三次进化与终局之战原创.html"]
---
# F-Harness

F-Harness 是 [[anthropic]] 提出的三角色分工机制，作为 [[harness-engineering-李伟山版]] 的核心实践案例之一。

## 三角色分工

| 角色 | 职责 |
|------|------|
| **Planner** | 规划任务分解 |
| **Generator** | 执行代码生成 |
| **Evaluator** | 独立审查产出质量 |

Evaluator 与 Generator "完全独立"是 F-Harness 解决 [[AI自恋问题]] 的关键设计。

## 设计动机：AI 自恋问题

[[AI自恋问题]]——AI 倾向于给自己的 Bug 和产出打高分的系统性偏差。[[anthropic]] 的 Claude.ai 克隆界面实验证明单 Agent 存在三大问题：

1. **中途遗忘**：执行过程中丢失早期上下文
2. **虚报完成**：声称测试通过但实际未通过
3. **自评过度乐观**：对自己的产出质量打高分

F-Harness 通过独立 Evaluator 角色解决自评不可信问题。

## 成本质量权衡

文章给出了完整的量化对比数据：

| 维度 | 单 Agent 模式 | F-Harness 三 Agent 模式 |
|------|-------------|----------------------|
| 耗时 | 约 20 分钟 | 约 6 小时（20x） |
| 成本 | 约 $9 | 约 $200（22x） |
| 输出质量 | 逻辑残缺，勉强可用 | 生产环境级别，逻辑完整 |

20 倍时间代价 + 22 倍成本代价换来质的飞跃。

## 与 [[可靠性边界论]] 的关联

文章明确论断：当任务复杂度超过单 Agent 的可靠性边界时，多 Agent 协作的 F-Harness 是唯一可行的工程解法。

## 与现有 Wiki 概念的关联

- F-Harness Evaluator 与 [[高阶模型审查低阶模型]]（美团）形成跨来源验证体系——两者均通过独立审查机制解决 AI 自评不可信问题
- F-Harness 三角色分工与 [[多agent角色思维隔离]]（Specflow）、[[多agent逆向工程初始化]]（有赞共享技术）高度呼应
- F-Harness 的 22 倍成本代价与 [[脚手架优于模型]]（[[zhiyuanfu]]：+50%成本+200%效果）的成本效益叙事存在量级差异，需关注

## 开放问题

- F-Harness 是否为 Anthropic 官方术语还是本文作者概括，有待考证
- $200 成本在常规开发中的可行性
- Evaluator 与 Generator "完全独立"的技术实现方式未展开