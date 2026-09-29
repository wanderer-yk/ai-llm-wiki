---
type: concept
title: 声称完成 vs 验证完成
tags: [agent-system, 验证机制, harness-engineering]
related: [harness-engineering-李伟山版, AI自恋问题, f-harness, 验证闭环, 验证门禁化, pre-pr机制]
created: 2026-07-20
updated: 2026-07-20
sources: ["[202605201731]干货从PromptContext到Harness工程的三次进化与终局之战原创.html"]
---
# 声称完成 vs 验证完成

"声称完成"vs"验证完成"是 [[李伟山]] 文章中关于 Agent 验证机制的核心区分。

## 定义

- **声称完成（Claimed Done）**：Agent 自我报告"测试通过""任务完成"——不可信，因为 [[AI自恋问题]] 导致 AI 倾向于虚报完成
- **验证完成（Verified Done）**：通过工具链强制验证闭环确认的完成状态——可信赖

## 工程实现

[[openai]] 百万行代码实验中的验证闭环（Verification Loop）策略：

- Chrome DevTools 视觉验证
- 可观测性工具监控
- 强制 Lint / 自动化测试

通过这些工具链将"声称完成"升级为"验证完成"。

## 在四步实践路线图中的位置

这一区分是 [[四步实践路线图-李伟山]] 第三步"Agent 系统设计"的四个关键问题之一：

1. 跑偏约束
2. **"声称完成"→"验证完成"**
3. 单 Agent vs 多 Agent 切分
4. 可观测性监控

## 与现有 Wiki 概念的关联

- "声称完成 vs 验证完成"是 [[验证门禁化]]（vivo [[ding-junjie]]：验证从"建议检查"升级为硬性阻断规则）的前置概念
- 与 [[pre-pr机制]]（美团：AI 多轮自查前置的代码提交审查机制）形成跨来源验证体系
- 与 [[AI自恋问题]] 互为因果——AI 自恋导致"声称完成"不可信，需要"验证完成"机制

## 开放问题

- "验证完成"的具体技术实现方案——文章提出概念但未展开工程实现细节