---
type: concept
title: skill 组合成工作流
tags: [hermes-agent, skill, 展望, 工作流]
related: [hermes-agent, skill生命周期元数据, 动态skill生成, skill自动创建触发条件]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604230830]深入源码HermesAgent如何实现SelfImproving.html"]
---
# skill 组合成工作流

skill 组合成工作流是来源文章作者对 [[hermes-agent]] 的**展望方向**：当前 Skill 相互孤立，各管一段操作；作者设想未来 Agent 能自动识别**高频共现**的 Skill 并将其合成为一条工作流 Skill，示例为 `flask-k8s-deploy` + `nginx-reverse-proxy` → `full-stack-deploy`。作者将这一步称为从"记住"（记住单件事怎么做）到"思考"（理解事情之间的组合关系）的跃迁。

**⚠️ 属性标注：作者展望，非 Hermes 已实现特性，具体技术方案本文未给出。**

## 与相邻概念的层级区分

- [[动态skill生成]]（0424 飞樰文）：面向**单个** Skill 的自动生成——同一层级的"从无到有"；
- 本概念：面向**Skill 之间**的组合编排——更高层级的"由组合涌现工作流"；
- [[skill生命周期元数据]]：为组合判断提供"哪些 Skill 高频共现、哪些该淘汰"的数据基础。

## 关联

该方向与 [[skill多阶段检查点编排模式]] 等 Skill 设计模式（工作流 Skill 研究）形成呼应：人工编写的多阶段编排 Skill，可能是自动组合机制的先验参照。