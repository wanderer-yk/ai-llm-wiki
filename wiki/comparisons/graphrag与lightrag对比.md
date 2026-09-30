---
type: comparison
title: GraphRAG 与 LightRAG 对比
tags: [graphrag, lightrag, 知识图谱, rag选型, 对比]
related: [graphrag, lightrag, edge-2024-graphrag, guo-2024-lightrag, graphrag工程化四困境, lightrag三函数索引构建, lightrag双层检索范式, AI答疑助手, "[202604101635]AI答疑助手优化实践从RAG到LightRAG的全链路升级"]
created: 2026-09-30
updated: 2026-09-30
sources: ["[202604101635]AI答疑助手优化实践从RAG到LightRAG的全链路升级.html"]
---
# GraphRAG 与 LightRAG 对比

本对比页基于 [[跨端技术团队]] 同批 44 篇文档双栈实验（一手实测口径，量化数据为作者粗粒度自报：快"数倍"、分钟级→秒级等，无精确数值）。

## 五维度实测对照

| 维度 | GraphRAG | LightRAG |
|---|---|---|
| 索引构建 | 四步流水线（TextUnit 分块+LLM 抽取→图合并→Leiden 社区发现→社区摘要），GPT-4 级别依赖，44 篇文档"费用不菲" | R/P/D 三函数（键值对替代社区摘要），省社区开销，同批 44 篇快"数倍" |
| 查询模式 | Local Search（具体问题）+ Global Search（社区摘要 Map-Reduce，一次查询 5 分钟以上） | 双层检索同时触发+合并去重，查询分钟级→秒级 |
| 增量更新 | 不支持（"几乎是灾难性的"：社区边界重划→摘要重生成，实践中多数团队定期全量重建） | 三步局部增量（R→D→P），实测耗时与 API 调用远小于全量重建 |
| 回答质量 | 跨文档综合与全局问题最强（Global Search）；Local Search 面向具体问题 | 具体问题 ≈ GraphRAG Local Search；全局问题逊于 Global Search（无社区摘要加持） |
| 场景适配 | 高成本、极致质量的**离线分析** | 实时响应、频繁更新的**在线答疑**；80%+ 具体问题场景够用 |

## 结论（来源文章总结章）

- GraphRAG **验证了图结构增强检索的有效性**；
- LightRAG **证明了该路线可以工程化落地**；
- 两者是互补的场景定位关系，而非简单的优劣关系。

## API/token 成本

实验中 LightRAG 的 API 调用与 token 消耗"大幅减少"（具体对比图表内数值不可读，需回源查图）。

## 前沿方向（来源文章综述，未实践）

- **MiniRAG**：图增强 RAG 在小模型（SLMs）上的尝试——异构图索引（文本片段节点+语义概念节点）+拓扑增强检索，面向端侧等资源受限场景；
- **Agentic RAG**：自主决策检索策略的智能体，见 [[agentic-rag]]。