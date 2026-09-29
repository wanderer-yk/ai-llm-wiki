---
type: concept
title: Plan-and-Execute 模式
tags: [ai-agent, agent模式, 规划, 结构化工作流]
related: [react-agent, reflection模式, agent-loop]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604131736]详尽地带你从零开始设计实现一个AIAgent框架.html"]
---
# Plan-and-Execute 模式

AI Agent 三大基础模式之一。学术溯源来自 Plan-and-Solve (2023) 论文。

## 核心思想

先制定多步计划（Plan），再逐步执行（Execute）。与 [[react-agent|ReAct]] 的"边想边做"不同，Plan-and-Execute 采用"先想好再做"的策略。

## 适用场景

- 长期复杂任务，需要全局视野
- 步骤间有强依赖关系的任务

## 局限性

- 缺乏动态调整能力：一旦计划制定，难以根据中间结果灵活修正
- 对初始规划的准确性要求高

## 与其他模式的关系

三大模式形成互补：
- [[react-agent|ReAct]]：边推理边行动，灵活但缺乏全局规划
- **Plan-and-Execute**：先规划后执行，有全局视野但缺乏动态调整
- [[reflection模式|Reflection]]：通过反思改进决策，可叠加在前两者之上

实践中，主流框架往往是这三种模式的组合使用。[[agent-loop|Agent Loop]] 的 While 循环机制天然兼容所有三种模式。