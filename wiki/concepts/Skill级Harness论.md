---
type: concept
title: Skill 级 Harness 论
tags: [harness, skill, 层级, 方法论]
related: [Harness六大核心部分, Harness核心价值三元组织论, harness-engineering, 生产级skill, skill-command-mcp三层架构, web-video-presentation]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202605090830]Harness实践让Agent自动制作知识讲解视频.html"]
---
# Skill 级 Harness 论

本文 2.7 节"回过头看"提出的层级判断：web-video-presentation 的 Skill 设计与 OpenAI/Anthropic 为自己搭的 Harness **本质无区别**，区别仅在层级——后者是工业级运行系统，前者是 **Skill 级协作协议**。

## 两个核心论断

1. **Skill 的工程系统定义**："Skill 就是把复杂的内容生产流程，拆成一个有流程、有状态、有检查点、有自检、有恢复机制的工程系统"，即给 Agent 搭"一条可重复执行的轨道"。
2. **误区澄清**：做 Harness 不一定要从零搭 Agent——"用 Skill 做好一个垂直开发工作也是在搭 Harness"。Harness 存在从 Skill 级协作协议到工业级运行系统的层级谱系。

## 与既有概念的口径差异（非矛盾）

爱奇艺 [[harness-engineering]] 采用重工程口径（五要素工程化约束、多源分治、执行语义冻结），本文则证明六核心可以以纯 Skill（文档+流程约定）形式落地。两者是同一方法论的层级谱系差异，而非对立证据，对照详见 [[ConardLi六核心与爱奇艺Harness五要素对比]]。

该论断与 [[生产级skill]]（腾讯）、[[skill-command-mcp三层架构]] 相互印证：Skill 是可复用 SOP 的工程化封装，也是 Harness 的最小可行形态。