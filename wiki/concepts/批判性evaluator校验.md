---
type: concept
title: 批判性 Evaluator 校验
created: 2026-10-09
updated: 2026-10-09
tags: [evaluator, 质量校验, 多角色架构, 长程任务, harness-engineering]
related: [双轨校验, 跨模型评估, 自我说服效应, 高阶模型审查低阶模型, harness-engineering, 双通道输出设计]
sources: ["[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务.html"]
---
# 批判性 Evaluator 校验

批判性 Evaluator 校验是《Harness Engineering: 让 Coding Agent 可靠完成长程任务》7.1 全量 Code Review 场景中处理主观维度产出的校验方法：当产出物质量（如审查意见的好坏）无法程序化判定时，引入独立的 Evaluator Agent 以批判性视角评估执行者的结果。

## 三角色架构

1. **主 Agent**：负责编排与调度；
2. **subAgent**：执行审查与修复；
3. **Evaluator Agent**：批判性评估审查意见的质量。

## Prompt 设计要点

Evaluator 的 Prompt 要有挑战性语气，要求其"主动挑毛病而非寻找优点"，避免与执行者形成相互印证。

## 为什么必须独立会话

同一会话内，Agent 此前的推理历史会触发 [[自我说服效应]]，使其偏向自认正确；因此 Evaluator 必须是独立会话，必要时进一步跨模型（如 Sonnet 做 Code Review、GPT 做置信度 Grader、结果交回 Sonnet 修复），即 [[跨模型评估]]。

## 与其他校验机制的谱系

- 与 [[双轨校验]] 构成完整校验体系：程序化校验覆盖客观维度（编译通过、JSON 可解析、schema 符合，零 Token、结果确定、可无限重复），批判性 Evaluator 覆盖主观维度；
- 与美团 [[高阶模型审查低阶模型]] 形成同型实践：用另一模型/独立角色担任质量裁判。

来源：[[sources/[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务|[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务]]（无糖可乐，百度Geek说，2026-04-08）。
