---
type: entity
title: 京东 Coding 平台
tags: [平台, 代码托管, 京东, ci/cd]
related: [京东物流, joyagent, 双rag架构, 智能代码评审系统]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602032040]基于知识工程JoyAgent双RAG的智能代码评审系统的探索与实践.html"]
---
# 京东 Coding 平台

京东 Coding 平台是京东内部的代码托管与协作平台，提供 MR（Merge Request）API 接口，支持 Webhook 触发、Diff 解析、评审意见写入等功能。

在 [[京东物流]]的 [[双rag架构|双 RAG 架构]][[智能代码评审系统]]中，Coding 平台是代码评审流程的入口和出口——通过 Webhook 接收 MR 事件，解析 Diff 内容供系统分析，最终将评审意见写回平台。
