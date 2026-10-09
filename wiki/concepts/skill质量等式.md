---
type: concept
title: Skill 质量等式
created: 2026-10-09
updated: 2026-10-09
tags: [claude-code, skills, prompt-engineering]
related: [skill流程非代码化, 提示词工程统一论, skill触发可靠性痛点, claude-code]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# Skill 质量等式

Skill 质量等式是由「Skill 流程非代码化」直接推出的三个等式（Q3 结论）：

1. **Skill 的质量 = 提示词的质量**——没有代码控制流兜底，SKILL.md 文本写作水平就是 Skill 的全部上限；
2. **流程保障 = 指令遵循率**——「标准化」能否兑现，取决于模型对注入指令的遵循程度；
3. **弱模型则流程乱**——同一个 SKILL.md 在不同能力模型上执行稳定性差异极大，流程稳定性是模型能力的函数而非 Skill 结构的函数。

## 方法论含义

Skill 工程化的核心投入应放在提示词写作（结构、步骤、校验点、边界条件），而非寻找「更工程化」的封装形式；评估 Skill 好坏的最直接方法是更换模型做对照实验，观察指令遵循率变化。写好 Skill 与写好 Rules 是同一种能力（[[提示词工程统一论]]）。
