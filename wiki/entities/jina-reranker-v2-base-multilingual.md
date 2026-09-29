---
type: entity
title: jina-reranker-v2-base-multilingual
tags: [model, reranker, multilingual, jina]
related: [两阶段重排序, 多维综合评分机制]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602041445]从回答者进化为研究员全面解析DeepResearch.html"]
---
# jina-reranker-v2-base-multilingual

jina-reranker-v2-base-multilingual 是一个用于精排阶段评估语义相关性的小模型，在 [[deep-research|Deep Research]] 的[[两阶段重排序]]机制中承担精排（第二阶段）的角色。

## 在 Deep Research 中的作用

在 URL 清洗的[[两阶段重排序]]流程中，粗排阶段快速筛选追求召回率，精排阶段则使用 jina-reranker-v2-base-multilingual 深度评估问题与 URL 文本信息之间的语义相关性，优化最终的 Top-K 结果。

该模型作为轻量级重排序器，与 LLM 滑动窗口算法可互为补充，共同确保交付给后续流程的 URL 质量。