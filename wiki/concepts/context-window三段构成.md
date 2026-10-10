---
type: concept
title: Context Window 三段构成
tags: [openclaw, context-engineering, 上下文窗口]
related: [context-engineering三支柱, openclaw, compaction双触发模式, 上下文窗口四要素]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践.html"]
---

# Context Window 三段构成

Context Window 三段构成是 [[openclaw]] 对上下文窗口内容的空间划分：①System Prompt；②完整对话 History（含工具调用）；③Skills 的 md 文件。其中 System Prompt 与 Skills 文件相对固定难调，**完整 History 是节省 token 的主要（也是唯一的）可优化空间**——压缩与修剪的全部工程努力都集中于此。

作者以"开卷考试"类比：保住最近必考的 45~50 页原文，将前面约 45 页压缩为摘要带进考场。该划分明确了 [[context-engineering三支柱]] 中 Compaction 与 Pruning 的作用边界，与 [[compaction双触发模式]] 衔接。注意区分：另一来源的 [[上下文窗口四要素]] 讨论的是窗口利用的四类内容要素，与本文的三段空间划分为不同抽象层。

来源：[[sources/[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践|[202604130830] 深度解析 OpenClaw]]。
