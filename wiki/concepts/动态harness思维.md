---
type: concept
title: 动态 Harness 思维
tags: [harness-engineering, 实践方法论, 工程师能力]
related: [harness衰变定律, harness-engineering-李伟山版, 四步实践路线图-李伟山, 工程师三职责]
created: 2026-07-20
updated: 2026-07-20
sources: ["[202605201731]干货从PromptContext到Harness工程的三次进化与终局之战原创.html"]
---
# 动态 Harness 思维

动态 Harness 思维是 [[李伟山]] [[四步实践路线图-李伟山|四步实践路线图]] 中的最高阶能力，被定义为"最难培养、也是最有价值的能力"。

## 核心能力

根据 [[harness衰变定律]]（模型能力越强，所需 Harness 越简单），工程师需要根据模型能力边界动态增减 Harness 约束强度——保持 Harness 恰当"薄厚"，不过度设计也不遗漏关键约束。

## 两个核心自检问题

1. **约束来源区分**：这个约束是因为模型能力不足而存在，还是业务逻辑本身需要？
2. **模型增强后简化预判**：如果下一版模型变强 20%，哪些 Harness 可以简化？

## 在实践路线图中的位置

动态 Harness 思维是 [[四步实践路线图-李伟山]] 的第四步（最高阶）：

1. Prompt 基础
2. [[context-engineering-李伟山版|Context Engineering]]
3. Agent 系统设计（[[声称完成vs验证完成|"声称完成"→"验证完成"]]、单 Agent vs 多 Agent 切分、可观测性监控）
4. **动态 Harness 思维**

## 开放问题

文章仅给出两个自检问题，缺乏系统性训练路径。"动态 Harness 思维"的实践训练方法有待补充。