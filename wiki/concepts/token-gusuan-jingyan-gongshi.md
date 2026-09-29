---
type: concept
title: Token 估算经验公式
tags: [工具, token估算, joyai, 经验公式]
related: [joyai, chatrhino-api, 分块安全阈值设计, 两级分块策略, 双rag架构]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602032040]基于知识工程JoyAgent双RAG的智能代码评审系统的探索与实践.html"]
---
# Token 估算经验公式

Token 估算经验公式是 [[京东物流]]团队基于测试数据反推的 [[joyai|JoyAI]] Token 计算规律。

## 公式内容

- **固定开销**：63 tokens
- **英文单词分级**：
  - ≤5 字符 = 1 token
  - 6-10 字符 ≈ 2 tokens
  - ≥11 字符 = 3 tokens
- **中文**：每 2 字符 1 token
- **空格**：不计 token

## 背景

JoyAI 未公开 tokenizer 细节，团队通过 [[chatrhino-api|ChatRhino API]]（`api.chatrhino.jd.com/api/v1/tokenizer/estimate-token-count`）进行测试数据反推，得出此经验公式。

## 应用

该公式用于 [[分块安全阈值设计]]和 [[两级分块策略]]，确保代码分块在 [[joyai|JoyAI]]的 128K 上下文窗口安全范围内处理。

## 与其他 Token 管理方案的关联

与 [[文档分块参数调优]]（有赞 chunk_size=600, chunk_overlap=100）形成对比——京东采用基于 Token 的分块策略，有赞采用基于字符的分块策略，两者都是针对各自模型特性的工程化适配。
