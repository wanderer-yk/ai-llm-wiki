---
type: concept
title: system-prompt动态组装机制
created: 2026-10-10
updated: 2026-10-10
tags: [claude-code, prompt-engineering, system-prompt, 缓存]
related: [prompt-context-harness三阶段, system-prompt优先级链, cacheScope分级缓存, system-reminder注入机制, claude-md四路径分层, 内外双版本提示词, 函数结果清理机制, 六大系统内置AgentTool, memdir结构化记忆系统]
sources: ["[202604200830]深度解析ClaudeCode在PromptContextHarness的设计与实践.html"]
---
# system-prompt动态组装机制

**system-prompt 动态组装机制**是 [[claude-code]] 构建系统提示词的工程方式：System Prompt 不是一份静态文本，而是在每次请求时由多个文件协同、按静态/动态积木式拼接的字符串数组，再发送给 Claude API。它体现了「提示词组装范式转变」——重点从写好一段提示词转为按条件组装多个模块。

## 六步组装流程

```javascript
QueryEngine.ask()
  → fetchSystemPromptParts()     // 获取默认 prompt + 用户上下文 + 系统上下文
  → buildEffectiveSystemPrompt() // 根据优先级选择最终 prompt
  → query()                      // 发送到 API
```

三大并行组件（`queryContext.ts` 的 `fetchSystemPromptParts()`）：`defaultSystemPrompt`（`constants/prompts.ts` 的 `getSystemPrompt()`）、`systemContext`（Git 状态）、`userContext`（CLAUDE.md + 当前日期）。

## 默认 Prompt 数组：静态 7 模块 + 边界 + 动态 11 模块

```javascript
[
  getSimpleIntroSection(),        // 身份介绍
  getSimpleSystemSection(),       // 系统行为规则
  getSimpleDoingTasksSection(),   // 任务执行指南
  getActionsSection(),            // 操作安全守则
  getUsingYourToolsSection(),     // 工具使用指南
  getSimpleToneAndStyleSection(), // 语气和风格
  getOutputEfficiencySection(),   // 输出效率要求
  "__SYSTEM_PROMPT_DYNAMIC_BOUNDARY__",  // 缓存边界线
  session_guidance, memory, ant_model_override, env_info_simple, language,
  output_style, mcp_instructions, scratchpad, frc, summarize_tool_results,
  numeric_length_anchors, token_budget, brief, // KAIROS 简报
]
```

动态模块按条件拼装，要点：会话特定指导（按启用工具生成）、自动记忆（`loadMemoryPrompt()`）、环境信息（含模型身份与知识截止 2025-05）、语言偏好、输出风格、MCP 指令、Scratchpad（会话专属临时目录替代 `/tmp`）、[[函数结果清理机制]] 与配套「重要信息转写」提示、长度锚点（仅内部版：工具调用间 ≤25 词、最终回复 ≤100 词）、Token 预算（用户目标为硬性下限，如 "+500k"/"2M tokens"，提前停止自动续跑）。

最终 Prompt 经 [[system-prompt优先级链]] 决策、[[cacheScope分级缓存]] 分块后发送。AgentTool 的动态 Prompt（[[六大系统内置AgentTool]]）是同一机制的子 Agent 实例。
