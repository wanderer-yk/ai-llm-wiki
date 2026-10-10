---
type: concept
title: RRF 排名融合算法
tags: [rag, 排序, 多路召回, rrf]
related: [查询改写召回路, 向量检索rag, 文档分块参数调优, RAG工具化双路径]
sources: ["[202604220830]AI实践基于SpringAI从0到1构建AIAgent.html"]
created: 2026-10-10
updated: 2026-10-10
---
# RRF 排名融合算法

RRF（Reciprocal Rank Fusion，排名融合）算法是[[aiagentdemo]]项目 RAG 流水线中融合多路召回结果的排序算法，核心公式为 **score(d) = Σ 1/(k + rank)，其中 k=60 为平滑常数**。其选型理由：「RRF 只看排名不看绝对分数，天然适合融合不同算法的结果」。

## 在项目中的实现

MultiRecaller 遍历所有 Recaller（语义 + BM25 + 查询改写三路），每路取 `PER_ROUTE_CANDIDATE_COUNT` 个候选后累加 RRF 分数，按分数降序取 topK：

```java
public List<Document> retrieve(String query, int topK) {
    Map<String, Double> rrfScores = new HashMap<>();
    Map<String, Document> keyToDocument = new LinkedHashMap<>();
    for (Recaller retriever : retrievers) {
        List<Document> results = retriever.retrieve(query, PER_ROUTE_CANDIDATE_COUNT);
        // RRF 公式：score(d) = Σ 1 / (k + rank)，k=60 为平滑常数
        accumulateRrfScores(results, rrfScores, keyToDocument);
    }
    return rrfScores.entrySet().stream()
            .sorted(Map.Entry.<String, Double>comparingByValue().reversed())
            .limit(topK)
            .map(entry -> keyToDocument.get(entry.getKey()))
            .toList();
}
```

## 完整检索参数链

**三路召回（语义 + BM25 + 查询改写）共 9 候选 → LLM Rerank 精排 → top 3 → 以「【参考资料 N】」模板拼接上下文**。设计动机：「单一召回策略总有盲区」——语义检索（SemanticRetriever，EmbeddingModel 余弦相似度）覆盖语义相近但措辞不同的查询，BM25（Bm25Retriever，TF-IDF 变体）覆盖精确关键词匹配，查询改写路（[[查询改写召回路]]）扩大语义覆盖面。

## 待核

`PER_ROUTE_CANDIDATE_COUNT` 具体数值未给出（若三路各 3 则恰为 9 候选，属推断）；`accumulateRrfScores` 的 key 归一化策略（keyToDocument 的 key 生成方式）未展示；LlmReranker 使用的 Rerank 模型、prompt 设计与调用成本未披露。