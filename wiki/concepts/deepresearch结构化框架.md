---
type: concept
title: DeepResearch 结构化框架
tags: [framework, report-generation, toc, deep-research]
related: [deep-research, deepsearch, 增量生成机制]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602041445]从回答者进化为研究员全面解析DeepResearch.html"]
---
# DeepResearch 结构化框架

DeepResearch 结构化框架是 [[deep-research|Deep Research]] 的上层组件，负责报告的撰写工作，与底层的 [[deepsearch|DeepSearch]] 引擎协同运作。

## 三步工作流程

1. **用户意图理解与 TOC 生成**——理解用户的研究需求，自动生成报告目录（Table of Contents）
2. **分章节执行**——按照 TOC 逐章节调用 DeepSearch 引擎进行搜索、阅读、分析
3. **全局整合**——将各章节内容进行拼接、润色和逻辑一致性检查

## 与增量生成机制的关系

DeepResearch 结构化框架在执行层面采用[[增量生成机制]]——监督者层级制定结构，执行者层级按序逐步生成各部分，以突破单次输出长度限制并提高连贯性。最后由监督者层级进行全局整合，消除跨章节的逻辑冲突和重复内容。