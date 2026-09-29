---
type: entity
title: Deep Research
tags: [ai-agent, deep-research, react, rl, llm]
related: [deepsearch, react-agent, langgraph, manus, race, fact]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602041445]从回答者进化为研究员全面解析DeepResearch.html"]
---
# Deep Research

Deep Research 是一种具备自主推理与深度搜索能力的 AI 研究系统/产品形态，标志着 AI 从被动信息搬运工向主动信息加工者（研究员）的跃迁。

## 本质定义

Deep Research 本质上是一个"带搜索能力的 [[react-agent|ReAct Agent]]"，但它不依赖提示词工程，而是通过**强化学习（RL）**真正学会了搜索策略——主动认知何时搜索、何时推理，并具备自我纠错能力。

## 双层架构

Deep Research 采用双层架构设计：

- **底层 [[deepsearch|DeepSearch]] 引擎**：搜索→阅读→推理的无限循环机制（`<think>`→`<search>`→`<information>`→`<think>`→`<answer>`），通过 RL 驱动搜索策略。
- **上层结构化框架**：[[deepresearch结构化框架|DeepResearch 结构化框架]]负责报告撰写，包含用户意图理解与 TOC 生成、分章节执行、全局整合三步。

## 三大工程化挑战

1. **URL 排序与清洗**：引入[[多维综合评分机制]]和[[两阶段重排序]]解决海量 URL 问题，精排阶段使用 [[jina-reranker-v2-base-multilingual]]。
2. **长文本全局上下文保留**：采用[[迟分算法]]，基于 [[jina-embeddings-v3]] 实现。
3. **输出长度限制与[[上下文腐烂]]**：采用[[双层级agent架构|双层级 Agent 架构]]，由 [[langgraph|LangGraph]] 状态机控制。

## 生成质量控制

- **[[race|RACE]] 框架**：自适应标准驱动评估（COMP/DEPTH/INST/READ+任务专属维度）。
- **[[fact|FACT]] 框架**：事实丰富性和引用可信度评估。
- **[[主动事实核查机制]]**：自动识别关键陈述→多源交叉验证→置信度评估。

## 与 Manus 的区别

Deep Research 是模型层面的原生智能进化（RL 掌握推理与搜索策略），而 [[manus|Manus]] 是高度工程化的 Agent 平台（整合大量工具，强调"调度"）。详见[[模型进化vs工程化调度]]。