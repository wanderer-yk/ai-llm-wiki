---
type: comparison
title: OpenClaw五层架构与四决策对比
tags: [openclaw, 架构分析, 跨团队对比, 对比]
related: [openclaw, openclaw五层架构, pi-agent工具集合口径三变体, 渐进式工具加载]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604280830]你不知道的Agent原理架构与工程实践.html", "[202604082000]从OpenClaw看Agent架构设计.html"]
---
# OpenClaw 五层架构与四决策对比

两篇来源分析同一系统 OpenClaw 但切面不同（review-95aaf420 立项）：千问篇（[[侑夕]]）给**架构分层**视角，vivo（[[wang-wenqian]]）给**设计决策**视角。

## 两框架并排

| 千问篇·五层架构 | vivo·四决策 | 映射关系 |
|---|---|---|
| Gateway / Channel 适配器 | （未展开——vivo 聚焦 Agent 体内部） | 五层独有：渠道接入与解耦层 |
| Pi Agent（主循环） | 决策④ 主循环设计（tool loop 节奏/终止条件） | 同一对象的宏观/微观两面 |
| 工具集（shell/fs/web/browser/MCP） | 决策② 工具加载（tools 缓存 vs prompt 内嵌 vs 渐进式）+ 决策③ 工具查找（全量注入/追加/子Agent/向量） | 五层讲"有什么工具"，四决策讲"工具怎么进上下文"——口径差异见 [[pi-agent工具集合口径三变体]] |
| 上下文+记忆 | 决策① 上下文管理（压缩/外置/检索） | 同域；vivo 补 Prompt Cache 约束（tools 变动→100K 缓存全失效） |

## 分歧与互补

1. **工具口径分歧实锤**：五层表含 browser/MCP，vivo 极简口径只有 4 件——粒度/层位差异（见三变体页裁定）。
2. **缓存视角独有**：vivo 的 Prompt Cache 约束（[[渐进式工具加载]]、claude-code 92% 缓存命中率靠"永不改 tools 列表"）为五层架构未覆盖的工程约束层。
3. **互补裁决**：分层图适合讲系统组成（新人入门），决策树适合讲工程权衡（落地选型）——同一对象的两种合法描述框架。
