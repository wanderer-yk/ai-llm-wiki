---
type: concept
title: SOP 即智能
created: 2026-06-22
updated: 2026-06-22
tags: [SOP, 理论基础, 流程编排, Agent Skills]
related: [agent-skills, 吴恩达, workflow优先于agent, 经典协同模式三式]
sources: ["[202602271818]AgentSkills与MCP一场被误解的替代战争.html"]
---
# SOP 即智能

**SOP 即智能**是 [[吴恩达]]（[[deeplearning-ai|DeepLearning.AI]]）提出的理念，认为通过结构化流程（Standard Operating Procedure，标准操作流程）可以大幅提升 AI 的产出质量。

## 核心论点

AI 的"智能"不仅来自模型的推理能力，更来自**结构化的流程编排**。将复杂任务分解为标准化的步骤序列，每步有明确的输入、输出和约束条件，可以显著降低 AI 犯错的概率。

## 与 Agent Skills 的关联

这一理念是 [[agent-skills]] 编排功能的理论支撑：
- Agent Skills 的"流程编排标准"特性正是 SOP 的技术实现
- Agent Skills 的"上下文感知能力"使 SOP 能够根据情境动态调整
- Agent Skills 的"透明可解释性"使 SOP 的执行过程人类可见

## 与 Wiki 既有概念的关系

- [[workflow优先于agent]] 是本理念在工程实践中的直接体现
- [[经典协同模式三式]] 模式1（分层架构）将 SOP 编排（Skills）与能力执行（MCP）分离
- 与 [[先易后难陷阱]] 的教训一致：Vibe Coding 跳过 SOP 设计导致后期返工