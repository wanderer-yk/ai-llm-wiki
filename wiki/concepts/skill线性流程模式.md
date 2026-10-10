---
type: concept
title: skill 线性流程模式（模式 1）
tags: [skill, 设计模式, 线性流程, 部署]
related: [vercel-deploy, skill决策树加按需加载模式, 模式选择决策树, 安全边界三原则, 防止llm偷懒4种武器, skill设计模式五加一模式对比]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# skill 线性流程模式（模式 1）

线性流程模式是 [[青斧]] 从 7 个顶级 Skill 中归纳的 5 种核心设计模式之一，适用于"先做 A，再做 B，最后做 C"式有明确步骤的操作，典型场景为部署、安装、迁移。代表案例为 [[openai-skills|openai/skills]] 的 [[vercel-deploy]]（77 行，7 个分析对象中最短），一句话精髓"最小但完整的 Skill 模板"。

**结构（五段式）**：标题 → Prerequisites → Quick Start → Fallback → Troubleshooting。

**关键技巧**（安全默认值/具体命令/超时提示/降级方案/负面指令）：默认选择最安全选项（"Always deploy as preview"）；每步给出具体 bash 命令；显式超时提示（600000ms 防部署中断）；提供降级方案；明确禁止行为（"Do not curl the deployed URL to verify"）。

**选择判据**：如果 Skill 的任务能用"先做 A，再做 B，最后做 C"一句话描述，就用线性模式。最小可用 Skill 模板即基于此模式（frontmatter → 核心原则+安全默认值 → Prerequisites → Steps → Troubleshooting 表）。对比其余模式见 [[skill设计模式五加一模式对比]]，选型见 [[模式选择决策树]]。
