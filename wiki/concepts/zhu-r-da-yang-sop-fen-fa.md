---
type: concept
title: "主R打样→SOP分发→全组并行执行"
tags: [重构, sop, 团队协作, 规模化]
related: [ling-pai-qi-zhong-gou, ren-ren-dui-qi-ren-ji-dui-qi, ai-you-hao-yan-fa-gui-fan]
created: 2026-06-08
updated: 2026-06-08
sources: ["用Agent评测思路管理AICoding31万行代码AI重构的实践.html"]
---
# 主R打样→SOP分发→全组并行执行

[[ling-pai-qi-zhong-gou|零排期重构]]的规模化执行模式，确保团队在不停止业务交付的前提下并行推进大规模重构。

## 三步流程

1. **主R打样**——核心开发者（主R）完成最复杂的重构案例，验证方案可行性
2. **SOP分发**——将打样过程沉淀为标准操作流程（SOP），分发给团队
3. **全组并行执行**——团队成员按SOP并行执行各自负责模块的重构

## 核心价值

- 解决"一个人重构完还是一群人一起重构"的规模化问题
- SOP确保质量一致性，降低每个人独立探索的成本
- 与[[ren-ren-dui-qi-ren-ji-dui-qi|人人对齐]]理念一致：先有标准（打样），再分发执行