---
type: entity
title: RACE
tags: [evaluation, framework, deep-research, quality-control]
related: [fact, deep-research, 多维度动态权重评估]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602041445]从回答者进化为研究员全面解析DeepResearch.html"]
---
# RACE

RACE（Reference-based Adaptive Criterion-driven Evaluation）是基于参考的自适应标准驱动评估框架，用于评估 [[deep-research|Deep Research]] 生成研究报告的质量。

## 四个顶层维度

RACE 框架首先确立四个通用评测维度：

- **COMP**（Comprehensiveness）——全面性
- **DEPTH**（Depth/Insight）——洞察力/深度
- **INST**（Instruction Following）——指令遵循
- **READ**（Readability）——可读性

## 自适应机制

在四个通用维度基础上，RACE 针对**每个任务自动生成 1-3 个专属评估维度**，并采用[[多维度动态权重评估|动态权重分配机制]]计算综合得分——评判 LLM 为每个任务动态计算各评测维度的权重，并生成定制化评测标准。

## 与 FACT 的互补关系

RACE 与 [[fact|FACT]] 框架相互补充，共同构成 Deep Research 能力的评估闭环：RACE 聚焦报告质量与标准遵循，FACT 聚焦事实丰富性和引用可信度。