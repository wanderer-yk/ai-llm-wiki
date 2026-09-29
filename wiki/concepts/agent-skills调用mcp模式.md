---
type: concept
title: Agent Skills 调用 MCP 模式
created: 2026-06-22
updated: 2026-06-22
tags: [协同范式, 架构设计, MCP, Agent Skills]
related: [agent-skills, mcp, skill-command-mcp三层架构, mcp工具定义范式, agent-skills工作流结构]
sources: ["[202602271818]AgentSkills与MCP一场被误解的替代战争.html"]
---
# Agent Skills 调用 MCP 模式

**Agent Skills 调用 MCP 模式**是 [[agent-skills]] 与 [[mcp]] 分层协同的核心范式：Skills 编排层在 conditional/data_requirements 步骤中以字符串列表引用 MCP 工具名（如 `mcp_order_history`），形成"**Skills 编排决策 + MCP 执行原子操作**"的协同关系。

## 协同结构

```
Agent Skills 层（声明式 YAML/Markdown）
  └─ workflow(steps)
       └─ conditional 分支 → 引用 MCP 工具名
            └─ MCP 层（Python 类 + @mcp_tool 装饰器）
                 └─ 执行原子操作（数据访问/模型调用/报告生成）
```

## 关键设计原则

1. **解耦协同** — Skills 通过自然语言/字符串引用 MCP 工具名，不直接调用代码
2. **关注点分离** — Skills 不硬编码技术细节，MCP 不包含业务规则
3. **变更独立性** — 修改业务流程不需要修改 MCP 服务，反之亦然
4. **权限分层** — Skills 层定义安全规则，MCP 层通过 permission 参数（read_only/write/sensitive_data）实现权限粒度控制

## 与 Wiki 既有概念的关系

- [[skill-command-mcp三层架构]] 的 Skill→Command→MCP 分层是本模式的实践形式
- [[mcp工具定义范式]] 描述了 MCP 层的 @mcp_tool 装饰器封装方式
- [[agent-skills工作流结构]] 描述了 Skills 层的 YAML 声明式定义方式
## 与 [[skill-command-mcp三层架构]] 的区别（duplicate 裁决注记）

两者同属"业务编排层调用能力执行层"范式，差异在于：

- **本模式**（京东牛潇）：Skills 编排决策 + MCP 执行原子操作，**无独立 Command 路由层**
- **[[skill-command-mcp三层架构]]**（腾讯 seanguo）：Skill 核心逻辑 → **Command 薄壳路由** → MCP Server 外部 API，多一层 Command 作为入口分发

判断依据：需要多入口统一分发/权限收敛时选三层架构；纯 Skills 内部编排引用时本模式更轻。二者不合并，保留各自来源语境。
