---
type: concept
title: MCP 敏感数据权限模型
created: 2026-06-22
updated: 2026-06-22
tags: [MCP, 权限控制, 安全, 细粒度权限]
related: [mcp, mcp工具定义范式, 敏感操作mcp隔离原则]
sources: ["[202602271818]AgentSkills与MCP一场被误解的替代战争.html"]
---
# MCP 敏感数据权限模型

**MCP 敏感数据权限模型**是 [[mcp]] 层实现细粒度权限控制的机制，通过 @mcp_tool 装饰器的 permission 参数定义多种权限级别。

## 权限级别清单

| 权限级别 | 适用场景 | 安全措施 |
|----------|----------|----------|
| `read_only` | 订单历史查询、系统状态获取 | 标准数据访问 |
| `write` | 客服备注创建、配置更新 | 权限校验逻辑 |
| `sensitive_data` | 信用报告获取 | 内置 `_sanitize_sensitive_data` 脱敏 |
| `model_execution` | 风险模型执行 | 通过 `model_registry` 加载预训练模型 |
| `document_generation` | 合规报告生成 | 数字签名 + 审计存档 |
| `deployment_write` | S3 上传部署 | CI 系统连接封装 |

## 设计理念

MCP 权限模型体现了 [[敏感操作mcp隔离原则]]：敏感操作必须通过 MCP 实现，[[agent-skills]] 仅包含公开业务规则。这一设计确保了即使 Skills 层配置被修改，敏感操作的安全边界也不会被突破。

## 与 Wiki 既有概念的关系

这一权限模型与 [[mcp工具定义范式]] 直接相关，为 [[agent-control-plane]]（Agent 系统控制平面：权限/边界/审计）提供了具体的技术实现方案。