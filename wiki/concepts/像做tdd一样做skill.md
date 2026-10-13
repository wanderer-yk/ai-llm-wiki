---
type: concept
title: 像做 TDD 一样做 Skill
created: 2026-10-13
updated: 2026-10-13
tags: [tdd, writing-skills, superpowers, 方法论, skill]
related: [writing-skills, superpowers插件, 合理化借口反驳, skill四类型分类法, 像做机器学习一样做prompt工程]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# 像做 TDD 一样做 Skill

"像做 TDD 一样做 Skill"是 Writing-Skills（[[superpowers插件]] 元技能）的构建方法论：将测试驱动开发的 RED-GREEN-REFACTOR 循环映射到 Skill 创建，先观察 Agent 无 Skill 时的违规基线，再以最小实现修复，再堵漏。

## TDD → Skill 创建映射表

| TDD 概念 | Skill 创建 |
|---|---|
| 测试用例 | 压力场景 + 子代理 |
| 生产代码 | Skill 文档（SKILL.md） |
| 测试失败（RED） | Agent 在没有 Skill 时违反规则（基线） |
| 测试通过（GREEN） | Agent 在有 Skill 时遵守规则 |
| 重构（REFACTOR） | 堵住漏洞，同时保持合规 |

## 三阶段要点

- **RED**：刻意构造时间压力+沉没成本+限时选项的压力场景（如"4 小时完成的功能 + 6:30 有晚餐 + 明天 9 点代码评审 + 没写测试，A 删除重来 / B 先提交 / C 现在写测试"），诱使无 Skill 的 Agent 违规并**记录合理化借口**——"现在你知道 Skill 必须防止什么了"
- **GREEN**：只针对基线中观察到的具体失败编写 Skill，**不为假设情况添加额外内容**（最小化原则）
- **REFACTOR**：对 Agent 新找出的每个合理化借口逐一添加明确反驳（见 [[合理化借口反驳]]），同时保持已有规则合规

## 方法论定位

与 [[像做机器学习一样做prompt工程]]（ML 式自动评测）和 [[双claude迭代开发模式]]（人工观察迭代）构成三条 Skill 开发方法论路线之一；适用于纪律执行型 Skill（[[skill四类型分类法]] 中测试最严格的类型）。
