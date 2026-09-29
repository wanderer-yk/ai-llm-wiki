---
type: concept
title: MCP 工具定义范式
created: 2026-06-22
updated: 2026-06-22
tags: [MCP, Python, 权限控制, 工具封装]
related: [mcp, agent-skills调用mcp模式, mcp敏感数据权限模型]
sources: ["[202602271818]AgentSkills与MCP一场被误解的替代战争.html"]
---
# MCP 工具定义范式

**MCP 工具定义范式**是 [[mcp]] 层封装原子操作的标准代码模式，采用 Python 类 + @mcp_tool 装饰器 + permission 参数实现权限粒度控制。

## 标准模式

```python
@mcp_tool(permission="read_only")
def get_order_history(customer_id):
    # 权限校验逻辑
    # 数据访问逻辑
    return structured_result

@mcp_tool(permission="write")
def create_support_note(ticket_id, note):
    # 权限校验
    # 数据写入逻辑
```

## 权限级别

MCP 支持多种 permission 级别实现细粒度权限控制：
- `read_only` — 只读数据访问
- `write` — 写入操作
- `sensitive_data` — 敏感数据访问（内置脱敏处理）
- `model_execution` — 专业模型调用
- `document_generation` — 文档/报告生成
- `deployment_write` — 部署操作

## 金融合规能力

在金融场景中，MCP 工具内置：
- `_sanitize_sensitive_data` — 敏感数据脱敏
- `crypto.sign_document` — 数字签名
- `audit_system.archive` — 审计存档

形成完整的合规链路：模板渲染→数字签名→审计存档。

## 与 Wiki 既有概念的关系

这一范式与 [[mcp敏感数据权限模型]] 直接相关，是 [[agent-skills调用mcp模式]] 中 MCP 层的实现方式。