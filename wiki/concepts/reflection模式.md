---
type: concept
title: Reflection 模式
tags: [ai-agent, agent模式, 反思, 自我改进]
related: [react-agent, plan-and-execute模式, agent-loop]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604131736]详尽地带你从零开始设计实现一个AIAgent框架.html"]
---
# Reflection 模式

AI Agent 三大基础模式之一，核心思想是 Agent 通过**反思改进决策**。涵盖三条学术脉络：

## 三条学术脉络

### 1. Reflexion（2023）
- 语言反馈强化学习
- Agent 在失败后生成自然语言反思，存入记忆供后续尝试使用

### 2. Self-Refine（2023）
- 迭代自我反馈
- Agent 对自己的输出进行批评并改进，循环迭代
- **量化效果**：所有评估任务中平均性能提升约 20%

### 3. CRITIC（2023）
- 外部工具验证 + 自我修正
- Agent 使用外部工具（如搜索引擎、代码执行器）验证自己的输出，发现错误后修正

## 与其他模式的关系

Reflection 可叠加在 [[react-agent|ReAct]] 和 [[plan-and-execute模式|Plan-and-Execute]] 之上，作为质量提升层。在 [[agent-loop|Agent Loop]] 中，Reflection 体现为 LLM 在循环中不断审查和修正自己的输出。