---
type: concept
title: Agent Skills 工作流结构
created: 2026-06-22
updated: 2026-06-22
tags: [YAML, 声明式配置, 工作流, Agent Skills]
related: [agent-skills, agent-skills调用mcp模式, 生产级skill]
sources: ["[202602271818]AgentSkills与MCP一场被误解的替代战争.html"]
---
# Agent Skills 工作流结构

**Agent Skills 工作流结构**是 [[agent-skills]] 定义业务流程的标准 YAML 声明式结构。

## 结构定义

```
skill:
  name: <技能名称>
  trigger:
    event: <触发条件>
  workflow:
    steps:
      - name: <步骤名>
        description: <描述>
        rules: <业务规则>
        conditional: <条件分支>
        prompt_template: <提示模板>
        constraints: <约束>
        data_requirements: [mcp_tool_name]  # 引用 MCP 工具
```

## 关键元素

- **trigger** — 定义事件驱动的激活条件
- **conditional** — 基于上下文（如 ticket_type）分支到不同的 MCP 工具集
- **rules** — 业务合规规则（如年龄≥18、收入验证、禁止歧视）
- **constraints** — 操作约束和安全边界
- **prompt_template** — 生成回复或决策的提示模板

## 实践示例

在客户服务工单处理场景中，Agent Skills 定义了：意图分类 → 条件数据需求（根据 ticket_type 路由到不同 MCP 工具集）→ 品牌标准回复生成的完整流程。

在金融风险评估场景中，Agent Skills 定义了：compliance_rules → assessment_steps → decision_logic → report_generation 的决策链路。