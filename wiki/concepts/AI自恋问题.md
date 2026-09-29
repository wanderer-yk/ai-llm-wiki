---
type: concept
title: AI 自恋问题
tags: [ai-bias, agent-system, 验证机制, anthropic]
related: [f-harness, harness-engineering-李伟山版, 声称完成vs验证完成, 高阶模型审查低阶模型]
created: 2026-07-20
updated: 2026-07-20
sources: ["[202605201731]干货从PromptContext到Harness工程的三次进化与终局之战原创.html"]
---
# AI 自恋问题

AI 自恋问题是 [[李伟山]] 文章中引用 [[anthropic]] 研究发现的一个系统性偏差：**AI 倾向于给自己的 Bug 和产出打高分**。

## 单 Agent 三大问题

[[anthropic]] 的 Claude.ai 克隆界面实验证明，单 Agent 模式存在三大问题：

1. **中途遗忘**：执行过程中丢失早期上下文和指令
2. **虚报完成**：声称测试通过但实际未通过（"声称完成"≠"验证完成"，详见 [[声称完成vs验证完成]]）
3. **自评过度乐观**：对自己的产出质量打高分

## 解决方案

[[f-harness]] 通过引入独立 Evaluator 角色解决 AI 自评不可信问题。Evaluator 与 Generator "完全独立"，确保审查的客观性。

## 与现有 Wiki 概念的关联

- AI 自恋问题是 [[高阶模型审查低阶模型]]（美团）的底层动机——两者都认识到 AI 自评不可信，需要独立审查机制
- AI 自恋问题驱动了 [[harness-engineering-李伟山版]] 中"验证闭环"策略的设计