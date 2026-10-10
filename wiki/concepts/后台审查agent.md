---
type: concept
title: 后台审查 Agent
tags: [agent, 异步, 复盘, hermes-agent]
related: [hermes-agent, 动态skill生成, 内外双路径自进化, 多agent交叉审核, agent轨迹]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践.html", "[202604230830]深入源码HermesAgent如何实现SelfImproving.html"]
---
# 后台审查 Agent

后台审查 Agent 是 [[hermes-agent]] 动态 Skill 生成流程的实现载体：回复完成后由 `run_agent.py` 中的 `_spawn_background_review` 异步 fork 出一个轻量审查 Agent，对刚完成的执行过程做复盘，形成“**前台即时响应、后台异步进化**”的运行模式——用户不被复盘流程阻塞，进化在后台持续发生。

## 三维度审查 Prompt

| 审查 Prompt | 审查内容 |
|---|---|
| `_MEMORY_REVIEW_PROMPT` | 记忆审查：提炼值得长期保留的经验/事实存入记忆库 |
| `_SKILL_REVIEW_PROMPT` | 技能审查：任务解决路径是否通用、值得固化为可复用 Skill |
| `_COMBINED_REVIEW_PROMPT` | 综合审查：反思优化空间与潜在错误模式 |

## 关联

- 复盘对象是 [[agent轨迹]]（完整任务对话记录），产出物是记忆条目与 Skill 文件包，构成 [[内外双路径自进化]] 的“经验记录→Skill 抽象”环节。
- “异步 fork 轻量实例做审查”与 [[多agent交叉审核]]（autoresearch 软件开发迁移）在“用另一个 Agent 审查执行 Agent”的结构上同构，但 Hermes 的审查是事后复盘而非事中/事后质量门禁。

## Hermes fork 实现细节（源码级，2026-04-23）

- **设计理由**：自省不应占用用户任务的 **attention budget**——后台化使 review 对用户完全无感。
- **fork 实现细节**（`run_agent.py:2665-2711` `_spawn_background_review`）：输出重定向 /dev/null；`max_iterations=8` 上限；禁用自身 nudge 防无限递归；与主 agent 共享 `_memory_store`；daemon 线程；审查提示词以 "Nothing to save." 收尾防交差式写入；**响应发送给用户之后才触发**。
