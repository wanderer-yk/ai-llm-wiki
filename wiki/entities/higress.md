---
type: entity
title: Higress
tags: [ai网关, 被测系统]
related: [ai网关压测项目, qoder, kiritomoe]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202601242318]QoderQuest10把执行交给AI把选择留给人类.html"]
---
# Higress

Higress 是一个 AI 网关系统，在本文中作为 [[ai网关压测项目]] 的被测目标系统出现。[[kiritomoe]] 使用 [[qoder]] Quest 对其进行性能评估，最终自动生成了《Higress AI 网关性能评估报告》。

由于 AI 网关以长连接和 SSE 流式响应为特征，传统的 API 网关压测指标（QPS、RT）不适用，评估重点关注 TTFT（首字延迟）、TPOT（单字生成时间）和并发连接数等 AI 特有指标。

Higress 在当前 Wiki 中仅作为被测目标出现，其技术架构细节待后续补充。