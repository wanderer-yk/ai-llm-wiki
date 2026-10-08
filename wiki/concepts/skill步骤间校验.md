---
type: concept
title: Skill 步骤间校验
tags: [工作流, validation, 质量门控]
related: [skill-body两种形态, skill迭代闭环, blocker-gate, 验证门禁化]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202603091800]打造高效易用的AgentSkill.html"]
---
# Skill 步骤间校验

工作流型 Skill Body 的关键机制：**每一步都带 Validation**，上一步输出满足条件才进入下一步；质量不达标可回退重做，形成可循环迭代的执行流。来源的 Sprint Planning Workflow 范例中，校验体现为量化门槛（如故事点上限 = 平均速率 × 0.85 缓冲）。

## 跨来源印证

"未通过校验不得进入下一步"的硬门控理念与以下实践同构：

- 爱奇艺 Specflow 的 [[blocker-gate]]：未完成需求对齐时强制阻断。
- vivo 丁俊杰的 [[验证门禁化]]：未通过验证/审批/沙盘/回滚方案均不能执行。

区别在于作用层级：步骤间校验作用于单个 Skill 的执行流内部，后两者作用于研发流程与业务执行环境。这也是 [[skill迭代闭环]] 中"评分/分析反馈"环节能在执行层落地的前提。
