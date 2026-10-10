---
type: concept
title: Agent发展三阶段
tags: [行业演进, agent架构, hermes-agent]
related: [hermes-agent, openclaw, claude-code, 内外双路径自进化, copilot到ai-agent范式转移]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践.html"]
---
# Agent发展三阶段

Agent发展三阶段是一个 Agent 能力演进的阶段划分框架：**早期 Agent（被动式一问一答）→ 自主 Agent（自主规划+调用工具）→ 自进化 Agent（Self-Evolving，执行中学习）**。来源文章以 [[openclaw]]、[[claude-code]] 为"自主 Agent"阶段代表，以 [[hermes-agent]] 为"自进化 Agent"阶段代表，并断言"从自主到自进化的跨越是 AI 系统架构演进的最显著特征"。

## 三阶段界定

| 阶段 | 特征 | 代表 |
|---|---|---|
| 早期 Agent | 被动式、一问一答 | — |
| 自主 Agent | 自主规划、调用工具；但每次执行从零开始，经验随会话结束消散 | [[openclaw]]、[[claude-code]] |
| 自进化 Agent | 执行中学习：Skill 动态沉淀 + RL 闭环训练，运行时间越长能力越强 | [[hermes-agent]] |

## 与既有框架的关系

- 与 [[copilot到ai-agent范式转移]] 的"Copilot → Agent"跃迁同向，但向前多推一步：自主并非终点，自进化是下一阶段
- 自进化阶段的实现依赖 [[内外双路径自进化]]（外层 Skill 沉淀 + 内层 RL 训练双轮驱动）
- 该划分是来源作者的观点性框架，属单一来源判断，宜与 [[ai工程量化效果声明追踪]] 中的行业声明一并交叉验证

## 开放问题

- "自进化 Agent"阶段的判定标准（Skill 沉淀是否充分、RL 训练是否必要条件）未严格定义
- Claude Code 是否具备部分自进化能力（如 Claude Code 的记忆文件积累）在该框架下如何归类，来源未讨论
