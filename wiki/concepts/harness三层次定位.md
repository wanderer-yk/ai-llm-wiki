---
type: concept
title: harness三层次定位
created: 2026-10-10
updated: 2026-10-10
tags: [harness-engineering, agent, claude-code]
related: [prompt-context-harness三阶段, harness-engineering, harness四要素, harness工程三部曲演进论, agent-control-plane, 验证门禁化]
sources: ["[202604200830]深度解析ClaudeCode在PromptContextHarness的设计与实践.html"]
---
# harness三层次定位

**harness 三层次定位**是飞樰对 Prompt / Context / Harness 三层工程的另一组凝练：Prompt Engineering = **What & How**（做什么、怎么做）；Context Engineering = **How Better**（怎么做得更好）；Harness Engineering = **How Controlled**（怎么可控地做）。Harness 的词源取「马具/千里马」隐喻——在大模型这匹千里马之外构建外部运行环境与约束机制。

## 手段与目标

| 维度 | 内容 |
|------|------|
| 手段 | 接口（Interface）、钩子（Hooks）、护栏（Guardrails） |
| 目标 | 约束、引导、检验、评估 |

在 [[claude-code]] 中的落地对应：`<system-reminder>` 系统级强提醒（Interface/引导）、钩子体系（Hooks）、Permission Engine + Sandbox + Verification Agent（护栏/检验，见 [[permission-engine三行为模型]]、[[sandbox按需隔离]]、[[verification-agent五大设计哲学]]）。

## 跨源关联

与爱奇艺 [[harness-engineering]] 五要素、[[harness四要素]]、[[agent-control-plane]]（控制平面）高度同构；「How Controlled」的使命表述与 vivo [[验证门禁化]] 的硬性阻断理念一致——不同团队独立收敛到「Agent 落地的关键在约束环境而非模型本身」。
