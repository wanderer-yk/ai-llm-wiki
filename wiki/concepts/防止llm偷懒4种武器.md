---
type: concept
title: 防止 LLM 偷懒的 4 种武器
tags: [skill, 提示词工程, 防偷懒, prompt技巧]
related: [test-driven-development, audit-context-building, vercel-deploy, 教学三种有效方式, 安全边界三原则, 自我说服效应, 强硬语气提高遵从率]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# 防止 LLM 偷懒的 4 种武器

"防止 LLM 偷懒的 4 种武器"是 [[青斧]] 在文章 4.1 节收拢的通用写作方法论，将分散在各模式中的技巧归纳为四条，各有原理与出处：

1. **强硬语气**——如 TDD Skill 的 "Delete it. Start over."；理由是 LLM 天然倾向"灵活变通"，强硬措辞抑制其自由发挥（参见 [[强硬语气提高遵从率]]）。
2. **借口反驳表**——预先列出并逐条反驳 LLM 的典型借口：TDD Skill 收录 12 种（Common Rationalizations），审计 Skill 收录 6 种；与 [[自我说服效应]] 概念强互证。
3. **量化阈值**——设定硬性最低标准强制深度，如审计 Skill 的"每个函数最少 3 个不变量、5 个假设"，使"做没做够"可客观判定。
4. **负面指令**——正文级明确禁止行为，如 vercel-deploy 的 "Do not curl the deployed URL to verify"（与 description 级的 [[负向触发说明]] 语境不同：一个管执行禁令，一个管触发边界）。

四种武器分别堵死"换做法""找借口""降低标准""绕过步骤"四类偷懒路径，与 [[教学三种有效方式]]（正面引导）和 [[安全边界三原则]]（边界兜底）共同构成 Skill 写作的通用技巧三角。
