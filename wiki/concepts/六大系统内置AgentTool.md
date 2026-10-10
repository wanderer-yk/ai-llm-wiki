---
type: concept
title: 六大系统内置AgentTool
created: 2026-10-10
updated: 2026-10-10
tags: [claude-code, multi-agent, subagent, agent-tool]
related: [verification-agent五大设计哲学, fork-sub-agent机制, 多agent设计四动机, 受控子Agent机制, 高阶模型审查低阶模型, agent权限边界清单, system-prompt动态组装机制]
sources: ["[202604200830]深度解析ClaudeCode在PromptContextHarness的设计与实践.html"]
---
# 六大系统内置AgentTool

**六大系统内置 AgentTool**是 [[claude-code]] 出厂自带的六个子 Agent 定义（另有隐藏的第七个 Fork Sub Agent，见 [[fork-sub-agent机制]]）。每个 Agent 以「工具白名单 + 模型档位 + 专属 System Prompt + 权限模式」四元组定义，是按任务分档调度的最小权限实践。

## 六 Agent 概览

| Agent | 定位 | 工具 | 模型 | 关键配置 |
|-------|------|------|------|----------|
| General-Purpose | 万能打工人 | `tools: ['*']` | 不指定则用默认便宜子模型 | 极简 System Prompt |
| Explore | 代码库侦察兵 | 只读（禁建临时文件） | 外部 Haiku / 内部主模型 | `omitClaudeMd: true`；速度优先，只消化搜索过程只带回结果 |
| Plan | 软件架构师 | 严格只读 | 继承父模型（架构设计需高质量思考） | 四步工作流；结构化输出含 Critical Files（3-5 个文件）模板 |
| Verification | 质量检验官 | 只读 + `/tmp` 临时脚本例外 | — | 详见 [[verification-agent五大设计哲学]] |
| Guide | 自我说明书 | — | Haiku | `dontAsk` 权限模式；知识域为 Claude Code CLI / Claude Agent SDK / Claude API；动态注入自定义技能/Agent/MCP 配置/用户设置 |
| Statusline Setup | 状态栏安装 | 仅 `Read`+`Edit` | Sonnet | 橙色"装修中"标识；能把 Shell PS1 转成 statusLine 配置 |

## 关键细节

- **Explore 效率门槛**：提示词要求"尽可能多地并行调用工具"；彻底程度三档 `"quick"` / `"medium"` / `"very thorough"`；常量 `EXPLORE_AGENT_MIN_QUERIES = 3`——不足 3 次搜索直接用 Glob/Grep 更快。
- **Plan 四步法**：理解需求 → 深入探索代码库（找已有模式）→ 设计解决方案（权衡与架构决策）→ 详细规划（步骤、依赖、风险）。输出模板含 `### Critical Files for Implementation`（列出 3-5 个最关键文件）。
- **General-Purpose System Prompt 原文**："You are an agent for Claude Code. Given the user's message, you should use the tools available to complete the task. Complete the task fully — don't gold-plate, but don't leave it half-done."（别镀金、别干一半）
- **子 Agent 委派指导手册**：AgentTool 的动态 Prompt 是给主 Agent 看的——有哪些下属（Agent 列表）、何时自己干/委派（When NOT to use）、怎么写工作说明、防瞎指挥（反模式警告）。

设计四动机见 [[多agent设计四动机]]；与 HermesAgent [[受控子Agent机制]] 构成跨框架对比；模型分档呼应 [[高阶模型审查低阶模型]]。
