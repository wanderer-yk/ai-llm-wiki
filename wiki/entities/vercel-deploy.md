---
type: entity
title: vercel-deploy（Skill）
tags: [skill, 部署, 线性流程, 安全默认值]
related: [openai-skills, skill线性流程模式, 安全边界三原则, 防止llm偷懒4种武器, 教学三种有效方式]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# vercel-deploy（Skill）

`vercel-deploy` 是 [[openai-skills|openai/skills]] 仓库中的一个 77 行 Skill（7 个分析对象中最短），是 [[青斧]] 归纳的线性流程模式（见 [[skill线性流程模式]]）的代表，一句话精髓为"最小但完整的 Skill 模板"。

结构为五段式：标题 → Prerequisites → Quick Start → Fallback → Troubleshooting。它同时是多项通用写作技巧的实例来源：安全默认值（"Always deploy as preview"——默认 preview 而非 production）；权限最小化（"Do not escalate the installation check"）；负面指令（"Do not curl the deployed URL to verify"——正文级明确禁止行为）；超时提示（600000ms 超时防止部署中断）；降级方案（Fallback 段）；每步附具体 bash 命令的教学方式（见 [[安全边界三原则]]、[[防止llm偷懒4种武器]]、[[教学三种有效方式]]）。
