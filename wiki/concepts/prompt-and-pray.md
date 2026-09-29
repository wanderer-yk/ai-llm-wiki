---
type: concept
title: Prompt-and-Pray（提示即祈祷）
tags: [vibe-coding, ai编程, 方法论]
related: [vibe-coding, agentic-engineering, 先易后难陷阱, ai编程幻觉]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604171736]从VibeCoding到AgenticEngineering重构后台开发全流程.html"]
---
# Prompt-and-Pray（提示即祈祷）

对 [[vibe-coding|Vibe Coding]] 本质的精准概括，由腾讯开发者 [[seanguo]] 提出：把需求扔给 AI 后祈祷不出错。

## Vibe Coding 的问题

- **无审查流程**：AI 生成的代码缺乏系统性审查机制
- **Commit message 混乱**：没有规范约束的提交信息
- **不可控**：生产环境下 AI 产出的质量无法保证
- **先易后难陷阱**：前期省掉的设计时间以 10 倍 debug 时间偿还（参见 [[先易后难陷阱]]）

## 与 Agentic Engineering 的对比

| 维度 | Vibe Coding | Agentic Engineering |
|------|-------------|---------------------|
| 核心机制 | 提示即祈祷 | 结构化流程 |
| 质量保证 | 依赖运气 | 依赖流程 |
| 人机关系 | 人全程驱动 | 人审核确认、AI 自主执行 |
| 适用场景 | 原型验证 | 生产环境 |

## 与已有概念的呼应

[[先易后难陷阱]]（zhiyuanfu 提出）描述了同样的认知路径：Vibe Coding 前期省掉的设计时间以 10 倍 debug 时间偿还。"大道如夷，而民好径"——seanguo 的 prompt-and-pray 是对这一现象的更精炼概括。

## 关联来源

- [[sources/[202604171736]从VibeCoding到AgenticEngineering重构后台开发全流程|[202604171736]从 Vibe Coding 到 Agentic Engineering：重构后台开发全流程]]