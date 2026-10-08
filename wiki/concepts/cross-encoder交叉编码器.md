---
type: concept
title: Cross-Encoder（交叉编码器）
tags: [重排序, Cross-Encoder, Attention, 检索模型]
related: [重排序rerank, 向量检索rag, embedding索引构建原理, rag全链路]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202605181736]RAG全链路技术详解.html"]
---

# Cross-Encoder（交叉编码器）

Cross-Encoder（交叉编码器）是与「预计算双塔向量」相对的检索匹配模型架构：把问题（Query）和「候选文档」拼在一起，作为一个长句子塞进模型（文中举例：gte-rerank），模型可以**同时看到问题和文档的每一个字**，从而捕捉双塔架构无法获得的细粒度交互信号。

## 工作机制

- Self-Attention 在 Transformer 每层使 Query 的每个 token 直接观察到文档的每个词；
- Cross-Attention 逐层捕捉极细微的匹配关系；
- 缺项惩罚示例：「针对华为手机的例子，强行检查，是不是华为？是不是低于 2000？有没有提到续航？如果缺了一项，它的得分会大幅跳水。」——多条件逻辑（2000 元以下、续航好的华为手机）在交叉编码下可被逐项强校验，而预计算向量难以平衡多条件。

## 与双塔架构的对照

| 维度 | 双塔向量召回 | Cross-Encoder 重排 |
|------|------------|-------------------|
| 计算时机 | 文档向量离线预计算，与 Query 无关 | Query 与文档拼接后在线联合编码 |
| 交互粒度 | 仅在向量空间做相似度，表达文档平均含义 | token 级 Self/Cross-Attention 细粒度交互 |
| 成本 | 检索快、可海量 | 逐对计算，延时/算力高（见 [[重排序rerank]] 四缺点） |

## 关联

其数学基础与 [[embedding索引构建原理]] 中的 Self-Attention 计算同源（Q×K 打分→归一化→加权求和 V）。在 wiki 谱系中与 [[向量检索rag]]（双塔路径）构成「粗召回＋精重排」组合。注：本文仅以 gte-rerank 为例点名，未展开重排序模型清单。