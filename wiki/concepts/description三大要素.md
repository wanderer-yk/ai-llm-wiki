---
type: concept
title: Description 三大要素
tags: [description, 编写方法, 触发词]
related: [description触发准确性权衡, 负向触发说明, agent-skill, skill触发评测方法]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202603091800]打造高效易用的AgentSkill.html"]
---
# Description 三大要素

好的 Description 需同时回答三个问题：

1. **能做什么**——核心价值是什么。
2. **核心能力**——具体包含哪些能力。
3. **激活条件**——用户说什么话、做什么操作时触发。

## 正面案例（逐字保留）

```makefile
# 清晰、具体、包含触发短语
description: >
  分析 Figma 设计稿并生成开发交付文档。当用户上传 .fig 文件、
  要求"设计规范"、"组件文档"或"设计转代码交付"时使用。

# 明确的服务边界和触发词
description: >
  管理 Linear 项目工作流，包括迭代规划、任务创建和状态跟踪。
  当用户提到"迭代"、"Linear 任务"、"项目规划"或要求
  "创建工单"时使用。
```

## 反面案例（逐字保留）

```makefile
# 太模糊，几乎什么都能匹配
description: Helps with projects.
# 缺少触发条件，Agent 不知道什么时候该用
description: Creates sophisticated multi-page documentation systems.
# 过于技术化，没有用户视角的触发词
description: Implements the Project entity model with hierarchical relationships.
```

反面模式归纳：太模糊 / 缺触发条件 / 过于技术化而无用户视角触发词。防过度触发的补充技巧见 [[负向触发说明]]，触发信息的评测见 [[skill触发评测方法]]。
