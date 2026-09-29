---
type: concept
title: Skill 功能聚合
tags: [agent, 工具查找, skill, 知识缓存]
related: [agent架构四决策, 生产级skill, 三大武器库, skill-command-mcp三层架构]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604082000]从OpenClaw看Agent架构设计.html"]
---
# Skill 功能聚合

按功能维度组织工具的"说明书"：将多个工具的调用方法内聚在一起。不是单个工具，而是"如何组合使用多个工具完成某类任务"的知识封装。

## 解决的核心矛盾

**接口维度与功能维度矛盾**：工具查找时，工具定义在接口维度（一个 API 一个工具），但用户需求在功能维度上查找能力。

四种工具查找方式（全量注入、追加上下文、子 Agent、向量搜索/关键词检索）都未能解决这一根本性错配。

## 工作方式

示例：20 个数据库/缓存/存储工具（`pg_*`、`redis_*`、`s3_*`）→ 3 个 Skill：

- 数据库管理 Skill（psql + pg_dump/pg_restore 组合用法，含常见用法和注意事项）
- 缓存管理 Skill
- 存储管理 Skill

搜索空间从 20 压缩到 3，token 消耗从每次重新探索降低到 1 次搜索 + 加载 ≈ 几百 token。

## Skill 即工具知识缓存

Skill 是工具调用知识的 Cache：

- 把频繁使用的工具组合知识预先组织好，避免每次从头搜索和学习
- Agent 首次完成新类型任务后可自动整理成 Skill
- 使用越多，积累越丰富，未来效率越高——自我优化循环

Skill 不是替代其他查找方式，而是在它们之上提供缓存层——底层仍可用搜索/追加上下文加载，但搜索空间和加载量被功能聚合大幅压缩。

## 与 Wiki 已有概念的关系

- [[生产级skill]]（seanguo/腾讯）— 从可复用 SOP 封装角度定义 Skill，本文从工具查找效率角度定义。两者可能是同一概念的不同侧面：SOP 封装是 Skill 的内容，功能聚合是 Skill 的查找价值
- [[三大武器库]]（腾讯 binxiong）— Skills 是三大武器库之一，本文的 Skill 功能聚合理念为 Skills 层提供了工具查找效率的工程论证
- [[skill-command-mcp三层架构]]（seanguo/腾讯）— Skill（核心逻辑）→ Command（薄壳路由）→ MCP Server（外部 API），本文的 Skill 功能聚合可视为该三层架构中 Skill 层的深层设计原理
- [[sdd留痕进化论]]（zhiyuanfu）— "高频动作固化为 Skill"的理念与 Skill 自动积累的自我优化循环一致