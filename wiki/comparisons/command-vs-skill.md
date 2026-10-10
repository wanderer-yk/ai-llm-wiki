---
type: comparison
title: Command vs Skill
tags: [command, skill, prompt模板, 控制权分界]
related: [workflow与agent控制权分界, skill-command-mcp三层架构, anthropics-skills, agentskillsioagent-skills-开放标准, function-calling大地基论, 渐进式披露替代向量检索]
sources: ["[202604220830]AI实践基于SpringAI从0到1构建AIAgent.html"]
created: 2026-10-10
updated: 2026-10-10
---
# Command vs Skill

Command 与 Skill 是[[aiagentdemo]]项目中并存的两种 **Markdown 驱动的 Prompt 模板机制**。核心区别一句话：**「Command 是用户告诉 Agent 做什么，Skill 是 Agent 自己判断该做什么」——Command 提供确定性的快捷入口，Skill 提供智能化的能力扩展**。

## 文件格式对照

Skill 文件 = YAML Front Matter（name + description）+ Prompt 模板：

```markdown
---
name: summarize
description: 对用户提供的文本内容进行摘要总结
---
请对以下文本进行摘要总结，提取核心要点：
{{input}}
```

Command 文件 = 纯 Prompt 模板，文件名即命令名：

```markdown
请对以下代码进行 Code Review，从代码质量、潜在 Bug、性能、可读性等维度给出改进建议：
{{input}}
```

注册机制：SkillManager 启动扫描 `classpath:skill/*.md`，SkillTool 将每个技能转为 ToolCallback 注册；CommandManager 启动扫描 `classpath:command/*.md`，用户经 `POST /api/command/execute` 主动执行。

## 六维度对比表（4.3 节原表）

| 维度 | Command | Skill |
|------|---------|-------|
| 设计理念 | 用户快捷指令 | LLM 可调用的工具 |
| 文件格式 | 纯 Prompt 模板 | Front Matter（name + description）+ Prompt |
| 是否注册为工具 | ❌ 不注册 | ✅ 注册为 ToolCallback |
| 调用触发方 | 用户主动指定命令名 | LLM 根据 description 自主决策 |
| 执行路径 | 用户 → Controller → AgentCore | 用户 → AgentCore → LLM 决策 → SkillTool |
| 适用场景 | 用户明确知道需要什么功能 | 需要 LLM 理解上下文后智能判断 |

## 跨源定位

- **控制权分界的项目级落地**：该对比是 [[workflow与agent控制权分界]]（同公众号文章）思想在项目代码中的微观实现——Command 侧是确定性 Workflow（用户指定、直接执行），Skill 侧是 Agent 自主决策（LLM 依 description 判断）；亦与 [[skill-command-mcp三层架构]]（seanguo：Skill 核心逻辑 → Command 薄壳路由 → MCP 外部 API）的分层相互印证；
- **格式同构**：Skill 的 YAML Front Matter（name + description）结构与 [[anthropics-skills]]、[[agentskillsioagent-skills-开放标准]] 的 Skill 定义同构，说明「name+description 供 LLM 自主判断」已成事实标准；
- **确定性/智能化互补**：Command 确定性 / Skill 智能化的二分，与 [[意图识别前置门控]]（确定性门控）+ `knowledge_search` 工具（智能检索）的 [[RAG工具化双路径]] 在架构上同构。

## 待核

Skill 文件是否支持更多 front matter 字段；Command 与 Skill 文件是否支持热加载（当前均为启动扫描）未披露。