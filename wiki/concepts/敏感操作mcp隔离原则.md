---
type: concept
title: 敏感操作 MCP 隔离原则
created: 2026-06-22
updated: 2026-06-22
tags: [安全, MCP, 权限隔离, 敏感操作]
related: [mcp, mcp敏感数据权限模型, 真实工程三大误区, agent-control-plane]
sources: ["[202602271818]AgentSkills与MCP一场被误解的替代战争.html"]
---
# 敏感操作 MCP 隔离原则

**敏感操作 MCP 隔离原则**是 [[牛潇]] 提出的安全设计原则：敏感操作必须通过 [[mcp]] 实现，[[agent-skills]] 仅包含公开业务规则。

## 原则内容

所有涉及以下特征的操作必须封装在 MCP 层：
- **数据敏感性** — 信用报告、客户隐私数据、支付信息
- **写入操作** — 数据库写入、状态变更、配置修改
- **外部系统调用** — 部署操作、第三方 API 调用、资金操作
- **需要审计追踪** — 合规报告生成、审批流程

## 技术实现

通过 [[mcp敏感数据权限模型]] 的 permission 参数（sensitive_data/deployment_write/write）实现细粒度权限控制，MCP 工具内置：
- 权限校验逻辑
- 数据脱敏处理（`_sanitize_sensitive_data`）
- 数字签名（`crypto.sign_document`）
- 审计存档（`audit_system.archive`）

## 安全保障

这一原则确保即使 Skills 层配置被非技术人员修改，敏感操作的安全边界也不会被突破——因为 Skills 层只能通过 MCP 工具名引用能力，无法直接执行敏感操作。

## 与 Wiki 既有概念的关系

与 [[agent-control-plane]]（Agent 系统控制平面：权限/边界/审计）高度一致，为 Control Plane 的权限维度提供了具体的技术实现方案。