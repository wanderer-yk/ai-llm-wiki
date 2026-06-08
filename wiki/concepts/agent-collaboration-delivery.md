---
type: concept
title: Agent协同调用交付
tags: [Agent架构, 多Agent, MCP, 协同]
related: [context-engineering, mcp, safety-iron-rule, conversation-driven-vs-task-driven, perceive-think-act-loop]
created: 2026-06-08
updated: 2026-06-08
sources: ["颠覆传统意图规划上下文工程数据自迭代让企业智能办公助手效能跃升200V10.html"]
---
# Agent协同调用交付

[[context-engineering|上下文工程]]的第三大模块，办理类任务的必选路径。文章强调知识/数据注入无法覆盖办理类任务，必须通过Agent协同实现跨系统串联与多步骤流程。

## 两种核心协同模式

### 智能规划模式
- **架构基础**：[[mcp|MCP]]扩展架构
- **核心机制**：大模型作为调度大脑，自主规划工具调用链
- **适用场景**：流程灵活多变、有一定容错率的场景
- **典型案例**：请假流程（系统查询→条件判断→表单填写→最终确认）

### 精准路由模式
- **核心机制**：智能助手担任路由中枢，意图识别模块直接路由调用专用Agent
- **接口方式**：流式接口接收结构化结果
- **适用场景**：专业化服务的高效复用
- **权限设计**：[[safety-iron-rule|调用权限由提供方业务部门自行管控]]

## 与Wiki已有概念的关系

- 智能规划模式对应Wiki中的[[conversation-driven-vs-task-driven|任务驱动]]+[[perceive-think-act-loop|感知-思考-行动循环]]
- 精准路由模式对应意图驱动的直接调度
- [[safety-iron-rule|安全铁律]]与[[thought-process-as-first-class-citizen|思考过程作为一等公民]]共同服务于企业级可观测性和可控性