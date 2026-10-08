---
type: concept
title: Skill 评测集构建规范
tags: [评测集, 触发评测, 边界用例]
related: [skill触发评测方法, skill-body评测对照实验, 评测集优先于知识库]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202603091800]打造高效易用的AgentSkill.html"]
---
# Skill 评测集构建规范

触发评测评测集的构建标准：**16-20 条，分两组**（应触发 8-10 条 / 不应触发 8-10 条）。最有价值的用例是"近似场景"边界 case——教科书式正例和明显无关的反例均无检验价值。

## 两个常见陷阱

1. **query 太干净**：教科书式指令在真实场景几乎不存在，越像真人越有参考价值（应包含团队背景、项目规模、约束条件等噪声信息）。
2. **反例太容易**：应选共享关键词但实际需要别的工具、或触及该领域但上下文表明不该触发的边界 case。

## 示例（逐字保留，less-loader → PostCSS 场景）

```json
[
  {
    "query": "我们团队要移除 less-loader，把 .less 文件全部转成 PostCSS 方案。项目比较大有 200 多个 LESS 文件，有复杂的 mixin 嵌套，用哪种方式风险更低？",
    "should_trigger": true
  },
  {
    "query": "项目已经在用 PostCSS 了，现在想加 postcss-px-to-viewport 做移动端适配，postcss.config.js 不知道怎么写。",
    "should_trigger": false
  }
]
```

第二条反例的设计体现区分度：同样涉及 PostCSS 关键词，但上下文表明是加插件而非迁移决策。评测集的使用方式见 [[skill触发评测方法]]；"评测集比知识库本身更有价值"的跨来源共识见 [[评测集优先于知识库]]（小红书 PMO）。
