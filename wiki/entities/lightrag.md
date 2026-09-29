---
type: entity
title: LightRAG
tags: [graphrag, rag, 知识图谱, 检索增强生成, 货拉拉]
related: [microsoft-graphrag, pathrag, graphrag, 货拉拉, 双路检索上下文, graphrag三类实体设计]
created: 2026-06-25
updated: 2026-06-25
sources: ["[202603181415]从RAG到GraphRAG货拉拉元数据检索应用实践.html"]
---
# LightRAG

LightRAG 是一种轻量化的 Graph-based RAG 范式，以**轻量化**为核心突破方向。与 [[microsoft-graphrag]]（知识挖掘维度）和 [[pathrag]]（路径推理维度）并列为三种主流 Graph-based RAG 范式之一。

## 货拉拉选型

[[货拉拉]] [[大数据技术团队]]在对比三种 Graph-based RAG 范式后，最终选择 LightRAG 作为其元数据检索方案2.0的技术基础。选型理由为：从**复杂度、灵活性、可嵌入性**三个方面最适合货拉拉的元数据检索场景。

基于 LightRAG，货拉拉构建了完整的 GraphRAG 架构，包括：

- [[graphrag三类实体设计]]：表/字段实体（含跨表血缘）、业务术语/缩写词实体、同义词层
- [[双路检索上下文]]：Local Query Context（低级关键词→同义词扩展→混合检索+图关系）与 Global Query Context（高级关键词→Embedding→图实体）
- [[实体权重计算模型]]：manual_boost × 多因子加权排序
- 实体ID增量更新机制

## 量化效果

在货拉拉元数据检索场景中，基于 LightRAG 的 GraphRAG 方案实现了：准确率从 55%→78%、知识召回率 91%、TopK 命中率 90%、MRR 0.73。

## 技术特征

LightRAG 的核心设计遵循 [[graphrag]]的[[离线在线两阶段架构]]：离线阶段进行知识抽取和图构建（Chunking→Embedding→Vector DB，同时 LLM 知识抽取→实体关系→Graph DB），在线阶段执行向量检索+图检索双路检索并整合 Prompt 输入 LLM 生成。