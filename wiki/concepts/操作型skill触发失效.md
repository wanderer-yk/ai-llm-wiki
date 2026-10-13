---
type: concept
title: 操作型 Skill 触发失效
created: 2026-10-13
updated: 2026-10-13
tags: [skill, 触发失效, issue-514, 边界条件]
related: [模型驱动触发逻辑, description只写触发条件, skill-creator六大局限, ai工程量化效果声明追踪]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# 操作型 Skill 触发失效

操作型 Skill 触发失效是 [[模型驱动触发逻辑]] 的失效边界条件：GitHub Issue #514（anthropics/claude-plugins-official，报告者 jroy-poka，2026-03-04，截至成文仍 Open）的实证发现——

> "operational workflow skills show 0% recall regardless of description quality"

即"运行部署脚本""生成日报"类的操作型工作流 Skill 触发率恒为 0%，**与 description 质量无关**；触发取决于任务复杂度/专业性。模型能直接处理的任务永不触发查询 Skill，description 优化对这类 Skill 完全无效。

## 机制解释

模型驱动触发的隐含前提是"模型需要外部知识才会查 Skill"。当任务本身是简单动作执行（模型无需领域知识即可执行），触发查询的动机不存在，因此召回率与描述质量解耦。

## 与总结层声明的张力

来源总结章声明"写好 description 比写好指令主体更重要"（[[description只写触发条件]]），与本发现构成**双向约束张力**：description 质量决定"需要知识的任务"的召回与正文阅读完整度，但对"无需知识的操作型任务"完全无效。两说并存待裁决。

## 未决问题

操作型 Skill 触发失效的解法（强制触发/命令式调用等）来源未给出；Issue #514 后续待跟踪。
