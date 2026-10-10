---
type: concept
title: skill 循环迭代模式（模式 3）
tags: [skill, 设计模式, 循环迭代, tdd, 防偷懒]
related: [test-driven-development, obra-superpowers, skill接力棒循环模式, 防止llm偷懒4种武器, 教学三种有效方式, 安全边界三原则, reflection模式, 自我说服效应]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# skill 循环迭代模式（模式 3）

循环迭代模式是 [[青斧]] 归纳的 5 种核心设计模式之三，适用于在单次会话中反复执行"做→验证→改进"的场景，典型如 TDD、代码审查、设计评审。代表案例为 [[obra-superpowers|obra/superpowers]] 的 [[test-driven-development]]（371 行），一句话精髓"堵死 LLM 偷懒的所有退路"。

**结构**：Iron Law 铁律（置于开头、不可违反的核心原则）→ Red-Green-Refactor 循环体（RED → Verify RED → GREEN → Verify GREEN → REFACTOR → Repeat）→ Common Rationalizations 借口反驳表 → Verification Checklist 退出条件。

**关键技巧**：`<Good>`/`<Bad>` 对比标签教学；12 种借口反驳表（预先堵死 LLM 的典型逃避路径，与 [[自我说服效应]] 互证）；8 项 checklist 验证清单（质量达标才允许结束）；人类兜底（"ask your human partner"，不确定时交给人）。

**选择判据**：任务呈"做→验证→改进"循环形态。与 [[skill接力棒循环模式]] 的本质区别是状态存储位置：对话上下文（单次会话，分钟~小时）vs 外部文件（长期项目，天~周），详见两模式页的四维对比。其 Phase/Verify 交替与 [[reflection模式]] 互证。
