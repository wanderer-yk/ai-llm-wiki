---
type: concept
title: spec与任务系统分工
tags: [sdd, task-system, architecture, claude-code]
related: [spec与toolruntime分工, 规格驱动ai开发, 组织接入交付链路论, 多agent系统工程论, 执行收回论]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604150830]ClaudeCode源码拆解从启动到多Agent扩展层.html"]
---
# spec与任务系统分工

spec与任务系统分工指两者的天然互补关系：Spec 定义任务目标（交什么），任务系统定义任务生命周期（怎么持续、怎么收场）。它与 [[spec与toolruntime分工]]（Spec 管目标 / Tool Runtime 管行动）并列，构成完整的三件套分工：

- **Spec**：管目标——定义要交付什么、边界在哪
- **Tool Runtime**：管行动——定义怎样稳定地执行动作
- **任务系统**：管生命周期——定义持续执行如何被管理、追踪、恢复

该分工在文章结论段升维至组织层：SDD 与运行时自然汇合——前者给目标和边界，后者给运行和落地；两边都成立，Agent 才不只是 demo，而会变成团队能力（见 [[组织接入交付链路论]]、[[规格驱动ai开发]]）。
