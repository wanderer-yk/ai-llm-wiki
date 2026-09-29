---
type: source
title: "Source: [202603221522]AICoding的范式演进QoderKiroClaudeCode与HarnessEngineering.html"
created: 2026-06-22
updated: 2026-06-22
sources: ["[202603221522]AICoding的范式演进QoderKiroClaudeCode与HarnessEngineering.html"]
tags: []
related: []
---

# Source: [202603221522]AICoding的范式演进QoderKiroClaudeCode与HarnessEngineering.html

# Consolidated Long-Document Analysis

## Final Global Digest
### Summary
源文件《AI Coding 的范式演进：Qoder、Kiro、Claude Code 与 Harness Engineering》从产品形态、工作流设计与验证体系三维度对比 Kiro / Claude Code / Qoder 三款工具，并系统阐述 Harness Engineering 方法论与"模型 vs 框架"的路线之争。文章基于作者 kiritomoe 在 HiMarket 项目中的实践，最终给出"认同实践、警惕教条"的结论：模型是地基，Harness 是杠杆；代码正从目的变为媒介，开发者价值转移至意图定义与质量审核。

### Entities
- **kiritomoe** — 作者，"AI Coding 经验总结"系列第四篇
- **kirito的技术分享** — 微信公众号，IP 属地浙江
- **Qoder** — 作者主力工具，CLI 内核 + IDE 外壳；Editor + Quest 双模式
- **Kiro** — IDE 型 AI Coding 工具，三阶段 Spec 强制串行
- **Claude Code** — CLI 极致产品，"把缰绳完全交给模型"
- **Boris Cherny** — Claude Code 主创，主张秘方在模型里
- **Noam Brown** — OpenAI 研究员，认为脚手架终被强模型替代
- **METR** — AI 安全评测机构
- **Scale AI / SWE-Atlas** — 基准测试方
- **Jerry Liu** — LlamaIndex 创始人，强调上下文工程
- **Latent Space** — AI 工程社区
- **HiMarket AI 开放平台** — 作者实践 Harness Engineering 的真实项目
- **Opus 4.6** — Kiro Teams 版不限量高性能模型
- **阿里云百炼 Coding Plan / GLM-5 / Qwen Code / Kimi CLI / Gemini CLI** — 竞品或替代工具

### Concepts
- **AI Coding 范式演进** — 文章核心主题
- **vibe-plan-spec 三代分类法** — 按人介入次数分类，按场景选用
- **requirement-design-tasklist 三阶段 Spec** — Kiro 标准工作流
- **CLI 结构性竞争力** — CLI 对 IDE 四点优势
- **Anthropic 标准定义能力** — MCP/Skill/CLAUDE.md/Hooks 成事实标准
- **Harness Engineering** — 驾驭者工程，四大原则
- **Map not Manual** — AGENTS.md 导航地图式编写原则
- **知识嵌入而非外挂** — 规范与决策存放于代码仓库内部
- **机械验证而非人工检查** — 规范转化为阻塞式可执行验证
- **迭代自愈而非等待评审** — 自动验证修复与代码库 GC
- **Big Model vs Big Harness** — 模型能力与工程脚手架路线之争
- **Kiro-Claude Code 互补论** — Kiro 解决对齐，Claude Code 解决执行
- **代码即媒介** — 代码从目的变为媒介，核心价值在意图定义与审核

### Claims
- Harness Engineering 四原则是任何 AI Coding 工作流通用的底层基础设施。
- 模型厂商强调模型，框架厂商强调工程；真相在中间：模型是地基，Harness 是杠杆。
- Harness 的投入产出比递减；Spec 与基础验证闭环最具性价比。
- 没有银弹，应按任务规模（小修/中等/Feature）灵活选择模式。
- 代码正从"目的"变成"媒介"，开发者核心价值转移至意图定义与质量审核。

### Evidence
- OpenAI 用 Codex + GPT-5 构建百万行代码产品，佐证 Harness 可行性。
- METR 与 Scale AI 评测表明主流复杂脚手架相比基本脚手架无显著提升。
- Latent Space 实验显示仅改进框架可提升 15 个模型的编码能力。
- 作者在 HiMarket 中通过验证脚本串联 + Qoder Agent Browser 将 Agent 产出从"能编译"提升到"能跑通"。

### Contradictions
- 模型厂商主张"薄壳 + 强模型"，框架厂商主张"复杂工程带来提升"。作者指出将 Claude Code 的壳套在弱模型上体验断崖式下跌，印证模型决定性，但基础脚手架仍不可或缺。

### Open Questions
- 随着模型能力进一步提升，现有 Agent 脚手架（复杂工作流约束与验证机制）有多少会被模型内生自主性取代？

### Cross-Chunk Relations
- Chunk 7-10 铺垫工具层面的三阶段 Spec 与 CLI vs IDE 之争；Chunk 11-12 拔高至方法论本质，用 OpenAI 的 Harness Engineering 解释 Spec 与验证体系为何是 AI Coding 核心基础设施，并收拢至"模型是地基、验证是杠杆"的实践哲学。本 Chunk（12/12）为收尾，未引入新内容，仅强化最终价值论断。

## Per-Chunk Analyses
## Chunk 1/12
### 概要
本区块为微信公众号文章 `[202603221522]AICoding的范式演进QoderKiroClaudeCode与HarnessEngineering.html` 的 HTML 头部元数据，尚未进入正文内容。从 `<meta>` 标签可提取出文章标题、作者、TLDR 摘要等关键信息。

### 新增/更新的实体
- **kiritomoe** — 文章作者（微信公众号文章署名）
- **Qoder** — AI Coding 工具/产品（此前未在 Wiki 中出现，与 Kiro、Claude Code 并列对比）
- **Kiro** — AI Coding 工具/产品（此前未在 Wiki 中出现，与 Qoder、Claude Code 并列对比）
- **Claude Code** — 已有实体 `[[claude-code]]`，本文章将其作为三大对比对象之一
- **Harness Engineering** — 已有概念 `[[harness-engineering]]`，本文章将其纳入范式分析

### 新增/更新的概念
- **产品形态** — 文章分析维度之一（从 meta description 可见"产品形态、工作流设计和验证体系三个维度"）
- **工作流设计** — 文章分析维度之二
- **验证体系** — 文章分析维度之三
- 
