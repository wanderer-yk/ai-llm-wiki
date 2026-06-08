---
type: concept
title: 安全铁律
tags: [安全, 权限控制, 企业AI]
related: [agent-collaboration-delivery, thought-process-as-first-citizen, enterprise-intelligent-assistant]
created: 2026-06-08
updated: 2026-06-08
sources: ["颠覆传统意图规划上下文工程数据自迭代让企业智能办公助手效能跃升200V10.html"]
---
# 安全铁律

[[agent-collaboration-delivery|Agent协同调用]]中的关键设计原则：**所有涉及数据写入或真实操作的工具，执行前必须中断流程、强制获取用户明确授权**。

## 核心要求

- 写操作工具执行前必须中断自动化流程
- 强制弹出确认界面获取用户明确授权
- 不可跳过、不可自动确认

## 设计哲学

企业场景对数据安全和操作可控性有严格要求。安全铁律确保：

1. **可观测性**：用户对系统行为始终知情
2. **可控性**：关键操作需人工确认
3. **权责对等**：业务Agent的调用权限由提供方业务部门自行管控

## 与Wiki已有概念的关系

安全铁律与[[thought-process-as-first-class-citizen|思考过程作为一等公民]]共同服务于企业级可观测性和可控性需求。两者从不同维度保障系统的透明度和安全性。