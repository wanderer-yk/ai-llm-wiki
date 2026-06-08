---
type: concept
title: 技能维度组织（Skill-Based Organization）
tags: [工具查找, skill, 组织]
related: [skill, skill-as-knowledge-cache, agent-architecture-design]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# 技能维度组织

**技能维度组织** 是解决工具查找问题的方法论：按**功能维度**而非**接口维度**组织工具，将多个工具聚合为 [[skill|Skill]]。

## 解决的矛盾

传统工具查找的根本矛盾：

- **接口维度查找**（模型原生）：一个个 API 平铺
- **功能维度需求**（实际需要）：用户想要的是"能力"

两者不匹配导致模型在海量接口中难以准确选择。

## 效果

- 将搜索空间从数十个接口缩减为几个功能模块。
- 大幅提升查找准确率，降低 Token 消耗。

## 参见

- [[skill-as-knowledge-cache]]
- [[skill]]
- [[agent-architecture-design]]