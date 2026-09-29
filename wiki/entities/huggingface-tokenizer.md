---
type: entity
title: HuggingFaceTokenizer
tags: [工具, 分词器, huggingface, nlp]
related: [bge-reranker, bge-本地重排序方案, onnx-runtime]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602032040]基于知识工程JoyAgent双RAG的智能代码评审系统的探索与实践.html"]
---
# HuggingFaceTokenizer

HuggingFaceTokenizer 是分词器组件，配合 [[bge-reranker|BGE 重排序模型]]使用。

在 [[京东物流]]的 [[bge-本地重排序方案|BGE 本地重排序方案]]中，HuggingFaceTokenizer 负责将文本预处理为模型可接受的输入格式。
