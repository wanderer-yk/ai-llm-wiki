---
type: entity
title: jina-embeddings-v3
tags: [model, embeddings, long-context, jina]
related: [迟分算法]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602041445]从回答者进化为研究员全面解析DeepResearch.html"]
---
# jina-embeddings-v3

jina-embeddings-v3 是支持 8192 Token 长输入的 SOTA 向量模型，是[[迟分算法]]（Late Chunking）的基础。

## 在 Deep Research 中的作用

在 [[deep-research|Deep Research]] 处理长文本时，[[迟分算法]]首先使用 jina-embeddings-v3 对整个文档进行预编码以保留全局上下文语义，然后根据边界线索进行均值池化切分。这一过程确保提取的知识块既精准又连贯，有效解决了传统分块后的上下文丢失问题。

jina-embeddings-v3 的 8192 Token 长输入能力是实现"先整体编码再切分"这一迟分策略的关键前提。