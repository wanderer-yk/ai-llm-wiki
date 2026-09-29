---
type: concept
title: 原子Agent设计三原则
tags: [agent, 架构设计, 多agent, 小红书]
related: [pi-agent, agent三层商用架构, 小红书PMO团队, pmo-bp-agent]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202605111758]打造AI时代项目管理新范式小红书PMO团队的Agentic探索之路.html"]
---
# 原子Agent设计三原则

小红书 [[小红书PMO团队]] 在 2.0 阶段从实践中总结并沿用至今的 Agent 架构设计原则。

## 三条原则

### 1. 原子 Agent 必须自我闭环
原子 Agent 执行期间不依赖 Master Agent 再次介入。要么成功返回结果，要么明确失败原因。不返回模糊的中间状态。

### 2. 复合 Agent 由原子 Agent 组合
复合 Agent 的能力来源于原子 Agent 的编排组合，而非自身实现逻辑。

### 3. 复合 Agent 禁止嵌套
复合 Agent 不能互相嵌套，避免死循环和算力消耗。这一限制从 2.0 阶段坚持至今。

## 配套机制

### Master-Sub Agent JSON 协议
所有子 Agent 返回标准 JSON 结构体，Master Agent 据此自主决策处理。

### 统一通知能力剥离
子 Agent 不再各自发消息，统一通过消息发送接口通信，避免消息轰炸。

## 跨领域关联

- 与 [[pi-agent]] 的极简 4 工具设计哲学有理念共鸣——都追求原子化 + 自闭环
- 与 [[agent三层商用架构]] 中框架层的设计约束对应
- 与 [[小红书PMO团队]] 2.0-4.0 的 Master-Sub 架构演进一致