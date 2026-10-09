---
type: concept
title: Skill 流程非代码化
created: 2026-10-09
updated: 2026-10-09
tags: [claude-code, skills, 工作流]
related: [claude-code, skill提示词注入本质, skill质量等式, agent-skill知识包, api请求位置决定论]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# Skill 流程非代码化

Skill 流程非代码化是本文导读三问 Q3 的定谳结论：Skills 官方叙事中的「标准化工作流」**不是代码层面的流程化**——源码中没有任何代码逻辑（if-else、循环等）控制 Skill 的执行步骤；所谓标准化工作流就是**结构化 Markdown**，执行完全依赖模型的指令遵循能力。

## 论证要点

- Skill 的全部实现 = SKILL.md 文件 + 触发机制（[[skill提示词注入本质]]）；
- 「流程」存在于提示词文本的步骤描述中，不存在于运行时控制流中；
- 因此「弱模型则流程乱」：同样的 SKILL.md 在不同模型上执行稳定性差异极大。

## 立场一致性

该结论与既有 wiki 概念 [[agent-skill知识包]] 的定性立场一致——Skill 本质是注入给模型的知识/提示词包，而非可执行程序。措辞差异（本文「可复用提示词文件」vs 既有「知识包」）属表述不同，机制层面兼容。

## 直接推论

见 [[skill质量等式]] 与 [[提示词工程统一论]]。
