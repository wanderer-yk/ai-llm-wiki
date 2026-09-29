---
type: entity
title: BGE 重排序模型
tags: [模型, 重排序, rag, 向量检索, bge]
related: [bge-本地重排序方案, onnx-runtime, huggingface-tokenizer, 向量召回质量不均问题, 双rag架构, 京东物流]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602032040]基于知识工程JoyAgent双RAG的智能代码评审系统的探索与实践.html"]
---
# BGE 重排序模型

BGE 重排序模型（`bge-reranker-base.onnx`）是 [[京东物流]]在 [[双rag架构|双 RAG 架构]][[智能代码评审系统]]中本地部署的重排序模型，用于解决 [[向量召回质量不均问题]]。

## 技术栈

- **模型文件**：`bge-reranker-base.onnx`
- **推理引擎**：[[onnx-runtime|ONNX Runtime]]（微软开源跨平台推理引擎，Java API 调用）
- **分词器**：[[huggingface-tokenizer|HuggingFaceTokenizer]]

## 部署方式

采用 [[bge-本地重排序方案|本地部署]]策略，兼顾质量与成本。核心流程为：文本预处理→构建查询-文档对→ONNX 推理→相关性评分→降序排序。

## 工程实践注意

CentOS 7.9 环境下需手动编译高版本 cglib 依赖以支持 ONNX 运行时。

## 核心价值

解决向量检索"相似≠有用"的固有局限。高召回率带来噪声，BGE 重排序通过更精准的相关性评分过滤低质量召回，减少模型幻觉。
