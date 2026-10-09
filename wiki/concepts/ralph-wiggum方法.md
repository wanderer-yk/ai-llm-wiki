---
type: entity
title: Ralph Wiggum 方法
tags: [ralph-wiggum, 单agent循环, 盲循环, 方法演进]
related: [autoresearch, smallnest-autoresearch, 多agent交叉审核, 反馈驱动迭代, karpathy]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# Ralph Wiggum 方法

Ralph Wiggum 方法是 2025 年底流行的一种单 Agent 盲循环自动化方法：用一条 shell 循环反复把 PROMPT.md 喂给 claude 命令行，使编码 Agent 摆脱单次 chat 交互、持续自主运行。名字源于《辛普森一家》角色（此背景为公开常识，非本文内容）。

## 核心命令（原文）

```bash
while true; do cat PROMPT.md | claude; done
```

## 局限（本文论证）

它解决了 chat 交互的持续运行问题，但本质是**单 Agent 自我循环**：没有外部审核视角，质量全靠测试 backpressure 和 prompt 工夫，属于"盲循环"——不知道上一轮哪里不好，只是不断重试。

## 方法演进叙事中的位置

本文以它为基线构建演进链：传统人工（写代码→测试→修复，Issue 规模化后失效）→ vibe coding（[[claude-code]]/[[codex]]，人被绑在逐轮检查循环里）→ Ralph Wiggum（单 Agent 盲循环）→ Karpathy [[autoresearch]]（量化"什么是改进"）→ [[smallnest-autoresearch]]（双 Agent 交叉审核 + [[反馈驱动迭代]] 替代盲循环）。Ralph Wiggum 与后两者的本质差异即"有无外部审核反馈"。
