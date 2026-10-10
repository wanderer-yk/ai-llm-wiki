---
type: concept
title: skill能力声明对象
tags: [skills, capability-declaration, claude-code]
related: [skill功能聚合, 生产级skill, skill知识三层架构, fork-sub-agent机制, 动态能力面稳定内部对象]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604150830]ClaudeCode源码拆解从启动到多Agent扩展层.html"]
---
# skill能力声明对象

skill能力声明对象指 Claude Code 中 skill 的本质：不是野生 prompt 片段，而是轻量的结构化能力声明，约束工具权限、触发条件、执行上下文、模型偏好、推理力度、是否 fork 等运行参数。源码级证据为 `SkillDescriptor` 骨架：

```typescript
type SkillDescriptor = {
  description: string
  allowedTools: string[]
  whenToUse?: string
  model?: Model
  effort?: Effort
  hooks?: Hooks
  executionContext?: 'fork'
  agent?: string
}
```

跨源互证：

- `executionContext?: 'fork'` 与 [[fork-sub-agent机制]] 直接同构
- `allowedTools`/`model`/`effort` 字段与 [[生产级skill]]、[[skill知识三层架构]]、[[skill功能聚合]] 互证——「skill 即结构化声明」是跨团队共识

skill 与 MCP（协议接入）、plugin（能力组合包）同属扩展层，共同服从 [[动态能力面稳定内部对象]] 原则。
