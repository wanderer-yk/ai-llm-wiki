---
type: concept
title: Agent 自主维护图谱
tags: [图谱维护, LLM自检, 质量体系, 愿景, UModel]
related: [umodel, Entity+Log+Link三元组建模, 知识准入控制, 活文档机制, 多agent逆向工程初始化]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱.html"]
---
# Agent 自主维护图谱

"Agent 自主维护图谱"是《[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱》展望部分提出的**愿景**（无实测数据）：让 Agent 承担代码知识图谱的持续维护职责，解决图谱随代码演化而腐化的问题。

## 三要素（愿景设计）

1. **重评估标记**：代码变更后，对受影响的 LLM 推断关系（`INFERRED`）标记待重评估；
2. **定期巡检**：检测孤立实体、缺失关系、过期数据；
3. **Verify 质量体系**：对图谱质量做系统性评估。

## 问题定位

图谱的保鲜问题是所有知识层的共同痛点：[[deepwiki|DeepWiki]] 的 badge 全量刷新成本延迟高；有赞 [[knowledge-wiki]] 以六步更新闭环人工保障；腾讯 [[活文档机制]] 以 archive 命令自动同步。UModel 的增量构建（`ingest --incremental`）解决了确定结构的同步，但 `INFERRED` 语义层无法机械同步——Agent 自主维护正是针对这一残留问题。

## 与相关概念的关联

- 与有赞 [[知识准入控制]]（边界比内容更重要）形成互补：准入控制解决"什么能进图"，自主维护解决"进图后如何保鲜"。
- 与 [[多agent逆向工程初始化]]（Controller→Implementer→Reviewer）对照：后者解决初始建图质量，前者解决持续运维质量。
- 与 [[活文档机制]] 同构延伸：`ingest --incremental` 同步结构层（机械），Agent 巡检维护语义层（智能）。