---
type: entity
title: ONNX Runtime
tags: [工具, 推理引擎, 微软, onnx]
related: [bge-reranker, bge-本地重排序方案, huggingface-tokenizer]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602032040]基于知识工程JoyAgent双RAG的智能代码评审系统的探索与实践.html"]
---
# ONNX Runtime

ONNX Runtime 是微软开源的跨平台推理引擎。

在 [[京东物流]]的 [[双rag架构|双 RAG 架构]]中，ONNX Runtime 以 Java API 方式调用运行 [[bge-reranker|BGE 重排序模型]]，实现本地化的重排序推理。

## 工程实践注意

CentOS 7.9 环境下需手动编译高版本 cglib 依赖以支持 ONNX 运行时。
