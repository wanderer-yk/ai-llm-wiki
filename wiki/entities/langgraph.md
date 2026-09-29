---
type: entity
title: LangGraph
tags: [framework, state-machine, agent, workflow]
related: [双层级agent架构, deep-research]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602041445]从回答者进化为研究员全面解析DeepResearch.html"]
---
# LangGraph

LangGraph 是一个用于构建状态机的框架，在 [[deep-research|Deep Research]] 中用于实现对[[双层级agent架构|双层级 Agent 架构]]复杂流程的精确控制。

## 在 Deep Research 中的角色

Deep Research 的[[双层级agent架构|双层级 Agent 架构]]包含监督者层级（Planner，负责需求澄清、子主题分解、资源分配）和执行者层级（Workers，负责搜索、阅读、分析等具体任务），最终由 Aggregator 拼接润色。LangGraph 状态机保障这一复杂分工流程的执行有序性，确保各层级之间的状态同步与流程控制。