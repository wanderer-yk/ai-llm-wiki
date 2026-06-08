---
type: concept
title: 人机协作测试SOP
tags: [测试, sop, 人机协作, quality-assurance]
related: [pre-pr-ji-zhi, gao-jie-mo-xing-shen-cha-di-jie-mo-xing, quality-inspection-ai, small-model-beats-llm-in-classification]
created: 2026-06-08
updated: 2026-06-08
sources: ["用Agent评测思路管理AICoding31万行代码AI重构的实践.html"]
---
# 人机协作测试SOP

Human-in-the-loop五步法测试标准操作流程，核心原则是**人工主导、AI辅助**。

## 五步法

1. **建立范围**——人工定义测试边界
2. **风险分级**——人工评估各模块风险等级
3. **设计分组**——人工设计测试分组策略
4. **生成步骤**——AI辅助生成具体测试步骤
5. **验证覆盖**——人工验证AI生成的测试是否充分

## 路线A vs 路线B

团队探索了两条路线：

| 维度 | 路线A：AI全自动 | 路线B：人工主导AI辅助 |
|------|-----------------|----------------------|
| 全局业务认知 | 缺乏 | 人工提供 |
| PRD依赖 | 极度依赖 | 辅助参考 |
| 高危场景 | 容易漏掉 | 人工保障 |
| 边缘用例 | 发散大量无价值用例 | 人工筛选 |
| 结果 | **不可行** | ✅ 经QA团队确认后沉淀为SOP |

## 核心洞察

AI适合帮人"看全"问题（覆盖面），但"什么重要"（判断力）需人来主导。这与[[small-model-beats-llm-in-classification|精确分类中小模型碾压LLM]]共同印证"AI不取代判断，AI放大覆盖"的观点。