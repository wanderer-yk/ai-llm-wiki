---
type: entity
title: JoyAI
tags: [大语言模型, 京东, llm]
related: [chatrhino-api, 京东物流, 双rag架构, token估算经验公式, 分块安全阈值设计]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602032040]基于知识工程JoyAgent双RAG的智能代码评审系统的探索与实践.html"]
---
# JoyAI

JoyAI 是京东的底层大语言模型，上下文窗口为 128K。

## Tokenizer 特性

JoyAI 未公开 tokenizer 细节，需通过 [[token估算经验公式]]进行估算。官方提供 Token 计算端点 [[chatrhino-api|ChatRhino API]]（`api.chatrhino.jd.com/api/v1/tokenizer/estimate-token-count`）。

## 在双 RAG 架构中的角色

在 [[京东物流]]的 [[双rag架构|双 RAG 架构]][[智能代码评审系统]]中，JoyAI 作为底层大语言模型驱动评审意见生成。由于 128K 的上下文窗口限制，系统设计了 [[分块安全阈值设计]]：触发阈值 100K Token（80% 安全余量）+ 单块上限 60000 Token。
