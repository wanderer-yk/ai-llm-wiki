---
type: concept
title: Skills 四阶段运行流程
tags: [skills, claude, function-calling, 运行机制]
related: [skills-sub-agent包装论, skills两大未解问题, function-calling桥梁论, mcp与skills竞争关系论, 沈询, 生产级skill]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202602260830]AIAgent系列深入解析FunctionCallingMCP和Skills的本质差异与最佳实践.html"]
---
# Skills 四阶段运行流程

Skills 四阶段运行流程是[[沈询]]对 Anthropic Claude Skills 运行机制的完整拆解：初始化→发现→加载→执行，每一步都依赖 Function Calling。作者的核心论断：无 FC 则 Skills 无法工作，Skills 只是把"加载文档"封装为函数的巧妙应用。

## 四阶段

1. **初始化**：SKILL.md + 附带脚本 + 资料打包；元数据（名称 + description）在启动时载入上下文，每个约 100 token
2. **发现**：LLM 基于上下文判断是否需要调用某 Skill（基于请求内容匹配已加载的 skill_name 与 description），判断机制与 Function Calling 的工具判断同构
3. **加载**：通过 Function Calling 调用 `load_skill(skill_name)`，将对应 SKILL.md 的内容注入当前上下文，作为执行指令依据
4. **执行**：Claude 依照 SKILL.md 中定义的流程执行任务；过程中按需经 FC 调用 bash 执行附带脚本（如示例 `scripts/rotate_pdf.py`）、调用 read_file 读取所需资源文件，最后整合执行结果

## 运行流程图示（原文 code-snippet，原样保留）

```javascript
Claude 判断是否需调用某 Skill（基于请求内容匹配已加载的 skill_name 与 description）
↓
若需要，则通过 Function Calling 调用 load_skill(skill_name)
↓
将对应 SKILL.md 的内容注入当前上下文，作为执行指令依据
↓
Claude 依照 SKILL.md 中定义的流程执行任务
↓
在执行过程中，按需通过 Function Calling：
 • 调用 bash 执行附带脚本
 • 调用 read_file 读取所需资源文件
↓
整合执行结果
```

## 意义

该流程从机制层面证明 [[skills-sub-agent包装论]]：Skills 的全部运行时行为都落在 FC 的结构化调用之上，它没有引入 FC 之外的新执行原语；其结构性局限（description 不结构化、聊天式接驳）见 [[skills两大未解问题]]。
