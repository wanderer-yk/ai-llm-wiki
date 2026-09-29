---
type: concept
title: BGE 本地重排序方案
tags: [核心技术, 重排序, bge, onnx, rag, 成本优化]
related: [bge-reranker, onnx-runtime, huggingface-tokenizer, 向量召回质量不均问题, 双rag架构, 智能代码评审系统]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602032040]基于知识工程JoyAgent双RAG的智能代码评审系统的探索与实践.html"]
---
# BGE 本地重排序方案

BGE 本地重排序方案是 [[京东物流]]在 [[双rag架构|双 RAG 架构]]中为节省成本采用的重排序策略，通过本地部署 [[bge-reranker|BGE 重排序模型]]解决 [[向量召回质量不均问题]]。

## 技术栈

- **模型**：`bge-reranker-base.onnx`（[[bge-reranker|BGE 重排序模型]]）
- **推理引擎**：[[onnx-runtime|ONNX Runtime]]（Java API）
- **分词器**：[[huggingface-tokenizer|HuggingFaceTokenizer]]

## 核心流程（6 步）

1. **文本预处理** — 清洗和标准化输入文本
2. **构建查询-文档对** — 将评审上下文与候选知识文档配对
3. **ONNX 推理** — 调用 BGE 模型计算相关性
4. **相关性评分** — 输出每对查询-文档的相关性分数
5. **降序排序** — 按分数从高到低排列
6. **结果输出** — 返回排序后的知识文档列表

## 核心价值

兼顾质量与成本：
- **质量**：BGE 重排序模型提供比纯向量相似度更精准的相关性评估
- **成本**：本地部署避免 API 调用费用，适合高频代码评审场景

## 工程实践注意

CentOS 7.9 环境下需手动编译高版本 cglib 依赖以支持 ONNX 运行时。
