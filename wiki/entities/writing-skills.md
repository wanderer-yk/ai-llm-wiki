---
type: entity
title: Writing-Skills
created: 2026-10-13
updated: 2026-10-13
tags: [superpowers, skill, tdd, 元技能]
related: [superpowers插件, skill-creator, 像做tdd一样做skill, skill四类型分类法, description只写触发条件, 合理化借口反驳]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Writing-Skills

Writing-Skills 是 [[superpowers插件]] 框架中的**元技能**——教 Agent 如何创建新 Skill 的 Skill。它与 Anthropic 官方的 [[skill-creator]]（元工具）"目标相似，但方法论截然不同"：Writing-Skills 走 **TDD 式路线**（测试先行），Skill-Creator 走 ML 式自动评测路线。官方仓库：https://github.com/obra/superpowers 。

## 文件结构

```
writing-skills/
├── SKILL.md                          # 核心指令
├── anthropic-best-practices.md       # Anthropic 官方最佳实践摘要
├── persuasion-principles.md          # 说服心理学原则
├── testing-skills-with-subagents.md  # TDD 测试方法论
├── graphviz-conventions.dot          # 图表约定
├── render-graphs.js                  # 图表渲染脚本
└── examples/                         # 示例
```

## 方法论主体

- **TDD 全循环**（[[像做tdd一样做skill]]）：RED（无 Skill 压力场景下记录违规基线与合理化借口）→ GREEN（只针对观察到的具体失败最小化实现）→ REFACTOR（逐条添加 [[合理化借口反驳]] 堵漏）
- **四类型分类法**（[[skill四类型分类法]]）：纪律执行型/技术指导型/思维模式型/参考资料型，每类绑定专属测试方法与成功标准
- **description 关键发现**（[[description只写触发条件]]）："writing-skills 中最重要的发现之一"——description 只写触发条件，绝不要总结工作流程，否则 Agent 走捷径跳过正文
- **官方最佳实践转述**：[[skill简洁性原则]]、[[skill自由度分级]]、[[skill工作流与反馈循环模式]]

## 所属框架定位

Superpowers 定位术语为 Vibe Engineering（[[vibe-engineering]]），含 14 个可组合 Skill，四大理念：测试先行/系统化优于随机化/复杂度缩减/证据优于声明。在来源总结中与 Skill-Creator 并列为 Skill 生态"构建方法论"层。
