---
type: entity
title: Agent Skills
created: 2026-06-22
updated: 2026-06-22
tags: [AI架构, 技能封装, 声明式架构, 流程编排]
related: [mcp, skill-command-mcp三层架构, 三大武器库, 生产级skill, 能力vs行为二分法]
sources: ["[202602271818]AgentSkills与MCP一场被误解的替代战争.html"]
---
# Agent Skills

Agent Skills 是一种**模块化能力封装标准**，通过声明式、配置化方式定义 AI 代理在特定场景下的行为规范、决策逻辑和工作流程。

## 核心特性

1. **流程编排标准** — 将原子操作组合为完整业务流程
2. **上下文感知能力** — 根据历史和环境动态调整决策逻辑
3. **透明可解释性** — 决策路径对人类可见，增强信任
4. **轻量级集成** — 支持无服务部署，修改配置即生效
5. **组合式架构** — 支持嵌套、组合、复用

## 设计哲学

Agent Skills 的核心设计哲学是**业务价值导向**，关注决策逻辑、流程规范、上下文适应和人类协作。架构本质是**声明式**（配置即生效、无服务部署），可由产品经理和业务专家（非技术人员）编写维护。

与 [[mcp]] 的核心区别在于：Agent Skills 回答"怎么做才对"（Behavior），而 MCP 解决"能不能做"（Capability）。详见 [[能力vs行为二分法]]。

## 工作流结构

标准 YAML 声明式结构：`skill → trigger(event) → workflow(steps)`，每步含 name/description/rules/conditional/prompt_template/constraints。Skills 层通过引用 MCP 工具名完成能力调用，形成"Skills 编排决策 + MCP 执行原子操作"的协同范式。

## 与 Wiki 既有概念的关系

- [[skill-command-mcp三层架构]] 中的 Skill 层对应 Agent Skills 的定位（核心逻辑编排层）
- [[三大武器库]] 中的 Skills 是腾讯对 Agent Skills 概念的实践应用
- [[生产级skill]] 描述了生产环境中 Agent Skills 应具备的完整文件结构和版本管理
- [[skill功能聚合]] 与 Agent Skills 的"组合式架构"特性呼应