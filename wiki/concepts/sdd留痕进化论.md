---
type: concept
title: SDD 留痕进化论
tags: [sdd, 留痕, agent, 进化, 系统优化]
related: [zhiyuanfu, 24h打工人, 文件轮询架构, 数据自迭代, 活文档机制, agent可观测性六维度]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202605071734]十年老技术开发的AIAgent探索之路.html"]
---
# SDD 留痕进化论

[[zhiyuanfu]] 提出的核心观点：SDD 在 Agent 场景中的核心价值不是 debug 而是进化。留痕回答四个关键问题：

1. **看到了什么输入？**
2. **为什么做此判断？**
3. **Prompt 在哪失效？**
4. **哪些动作可固化为 Skill？**

## 留痕 → 观测 → 进化

"留痕只是起点，不是终点；observability 才是系统优化的闭环。" 留痕数据沉淀后，可通过 [[agent可观测性六维度]]（goal/step/tool/failure/recovery/cost）实现系统级优化，最终将高频动作固化为 Skill。

## 与其他留痕理念的关联

- 与 [[数据自迭代]]（马上消费）理念相通但路径不同——本文通过文件留痕驱动 Agent 能力进化
- 与 [[活文档机制]]（腾讯 binxiong）的"自动同步消灭僵尸 Wiki"形成互补——前者是进化机制，后者是同步机制