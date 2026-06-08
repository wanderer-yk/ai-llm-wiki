---
type: concept
title: Agent工作三原则
tags: [harness-engineering, agent, 工程条件]
related: [harness-engineering, harness-wu-yao-su, skill-based-organization, prompt-based-tool-injection]
created: 2026-06-08
updated: 2026-06-08
sources: ["别让AI瞎猜了用HarnessEngineering终结无限返工.html"]
---
# Agent工作三原则

[[harness-engineering|Harness Engineering]]提炼的agent工作基本约束，揭示了agent实际能力取决于工程条件而非仅靠模型能力。

## 三原则

1. **无法访问的知识 = 不存在**
   - agent只能使用项目文档、代码仓库、任务系统中可发现的信息
   - 隐性知识（存在于人脑中的经验、规则、历史教训）必须外化为可发现上下文

2. **无法执行的工具 = 没有**
   - agent只能调用明确注册且授权的工具
   - 工具边界必须明确定义，否则agent无法利用

3. **无法验证的目标 = 无法持续修正**
   - 没有可执行的验证入口，agent无法判断产出是否正确
   - 验证条件必须提前定义并固化

## 来源

原则源自 OpenAI Harness Engineering 框架（"agent能看到什么、能调用什么，决定了它实际能完成什么"），[[shu-ju-ku-tuan-dui|爱奇艺数据库团队]]在文章中进行解读。

## 与Wiki已有概念的关系

- 三原则映射到[[harness-wu-yao-su|五要素]]：知识访问↔执行依据，工具执行↔工具边界，目标验证↔验证反馈
- 与[[skill-based-organization|Skill维度组织]]和[[prompt-based-tool-injection|Prompt级工具注入]]存在关联——两者都是让agent"能访问到知识和工具"的不同实现路径