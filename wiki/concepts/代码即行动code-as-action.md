---
type: concept
title: 代码即行动（Code-as-Action）
tags: [code-as-action, codeact, AssistantAgent, agent范式, 代码生成, graalvm]
related: [AssistantAgent, GraalVM, CodeactTool工具体系, codeact架构, mcp, function-calling-mcp-skills本质差异对比, react-agent]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601150830]阿里AssistantAgent开源助力开发者快速构建答疑诊断智能助手.html"]
---
# 代码即行动（Code-as-Action）

代码即行动（Code-as-Action）是 [[AssistantAgent]] 的核心执行范式：Agent 不止于调用预定义工具，而是**通过生成和执行代码来编排工具、完成任务**，可在代码中灵活编排、组合多个工具实现复杂流程；该范式由 [[GraalVM]] 多语言沙箱提供安全执行底座（细节待外部验证）。

## 文内四重互证

1. 技术特性列表第 1 条："代码即行动"（chunk 5）。
2. 配置命名空间 `spring.ai.alibaba.codeact`——扩展模块配置均挂载于 codeact 之下（chunk 6、8）。
3. 学习模块流程图中包含"代码生成"环节，生成代码的执行经验被提取为经验（chunk 7）。
4. 动态工具模块命名为 CodeactTool 工具体系（chunk 8，见 [[CodeactTool工具体系]]）。

## 与既有概念的关系

- 与 [[codeact架构]]（UIUC 王星尧提出的"可执行代码统一 LLM Agent 行动空间"）属于**同一思想家族**：均以可执行代码作为行动表示，区别在于 CodeAct 是学术范式叙述，Assistant Agent 是将其产品化为 Spring 生态框架并配套沙箱、评估、经验学习等工程组件。
- 相对 function-calling 路线：代码即行动将"多工具编排"从模型逐轮决策转移为一次性代码生成，复杂流程的表达能力更强；工具生态兼容上，MCP Tools 与 HTTP API Tools 并列于 CodeactTool 体系（MCP 为一等公民），相关讨论见 [[function-calling-mcp-skills本质差异对比]]、[[mcp]]。
- 经验模块中的"ReAct 决策经验"（见 [[经验学习与快速响应]]）表明框架同时保留了 [[react-agent]] 式决策路径，代码生成与 ReAct 决策并存为两类可学习经验。
