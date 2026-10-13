---
type: concept
title: Skill 渐进式加载机制
created: 2026-10-13
updated: 2026-10-13
tags: [skill, 上下文工程, 渐进式披露, 上下文膨胀]
related: [skillmd格式规范, 上下文工程, 渐进式披露替代向量检索, 工具树按需加载, description只写触发条件]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Skill 渐进式加载机制

Skill 渐进式加载机制是 Skill 系统解决 **Agent 上下文膨胀问题**的核心机制：Skill 的内容分三层按需加载，而非一次性全量注入上下文。来源作者在总结章将其定性为"核心机制，解决了 Agent 系统的上下文膨胀问题"。该机制明确借鉴 UI/UX 渐进式信息披露策略。

## 三层结构

| 层级 | 内容 | 加载时机 |
|---|---|---|
| L1 元数据目录层 | name + description（以 XML 格式注入系统提示词，约 50-100 tokens，口径待核） | 常驻 |
| L2 指令层 | SKILL.md 正文（≤500 行） | 触发后按需读取 |
| L3 资源层 | references/、assets/ 等资源文件 | 指令执行中按需加载 |

## 量化声明

即使安装 20 个 Skill，初始加载仅 1000-2000 tokens；相比单体式提示词**上下文使用量减少约 90%**（口径待核，已登记 [[ai工程量化效果声明追踪]]）。

## 关联

- **Tool Wrapper 设计模式**（[[skill五种设计模式]]）是渐进式加载的设计模式化表达："SKILL.md 本身不包含完整规范，而是告诉 Agent 去哪里加载规范"
- 与 [[渐进式披露替代向量检索]]（有赞共享技术：目录结构即检索路径，YAML description 做摘要判断）和 [[工具树按需加载]] 构成同一思想的跨团队收敛
- 依赖 [[description只写触发条件]]：若 description 总结工作流，Agent 会跳过 L2/L3，渐进式加载即失效
