---
type: concept
title: 从Prompt到Harness的范式转换
tags: [harness-engineering, prompt-engineering, 范式转换]
related: [harness-engineering, prompt-kou-tou-chuan-tong-xian-jing, prompt-tuo-li-xiang-mu-xian-jing, spec-driven-development]
created: 2026-06-08
updated: 2026-06-08
sources: ["别让AI瞎猜了用HarnessEngineering终结无限返工.html"]
---
# 从Prompt到Harness的范式转换

[[harness-engineering|Harness Engineering]]理论论证的核心主线：从Prompt Engineering（文本级操作）到Harness Engineering（工程级操作）的范式跃迁。

## Prompt的两大结构性陷阱

1. **口头传统陷阱**：prompt知识集中于少数人，哪些话必须加、哪些规则不能漏变成隐性知识。不断加长prompt是短期有效、长期有害的策略
2. **脱离项目陷阱**：换上下文/工具入口，经验即丢失。prompt无法与项目结构绑定

## 五维度对比

| 维度 | Prompt Engineering | Harness Engineering |
|------|-------------------|-------------------|
| 背景 | 每次对话重复描述 | 项目入口docs（AGENTS.md） |
| 约束 | 提示词中罗列规则 | plan/规则/gate固化 |
| 格式 | 格式要求写在prompt | 协作协议/回写格式约定 |
| 验证 | 提醒跑测试 | 可执行验证入口 |
| 经验复用 | 复制旧prompt | 写回仓库/任务系统/PR |

## 核心论断

> "prompt解决的是这一轮怎么说清楚，harness解决的是项目里如何持续做对。"

- PE关注单轮回答质量（文本级操作）
- HE关注项目能否反复承接agent工作（工程级操作）
- 一次生成不构成工程能力，可持续能力来自一条能跑完的链路

## 与已有概念的关系

- [[ssot-dan-wen-dang-ce-lue|SSOT单文档策略]]（plan.md集中信息）直接回应了"prompt脱离项目"陷阱
- [[always-ji-bie-ai-rule|always级别AI Rule]]规范化尝试解决类似"口头传统"问题，但路径不同（规范约束 vs 工程安排）
- [[spec-driven-development|规格驱动开发]]的规格契约层消除了prompt中的猜测成分