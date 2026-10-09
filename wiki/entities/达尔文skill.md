---
type: entity
title: 达尔文.skill
tags: [达尔文skill, skill优化, autoresearch迁移, 花叔]
related: [花叔, auto-optimize-skill, autoresearch, smallnest-autoresearch, 硬性保护与软性保护, skill迭代闭环, skill评测三原则, skill-for-skill元技能自举]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# 达尔文.skill

达尔文.skill 是花叔开发的 Skill 优化工具，将 Karpathy [[autoresearch]] 方法应用于 Skill 开发领域：以"8 维总分"为量化指标驱动迭代优化，每轮优化暂停等待人工确认，并用 git revert 做硬性保护（指标退化即回滚）。其 8 个维度的具体构成本文未披露。

## 与另外两个 AutoResearch 实践的对比

| 维度 | 达尔文.skill | Karpathy 原版 | [[smallnest-autoresearch]] |
|------|------|------|------|
| 被优化资产 | Skill | LLM 训练代码 | GitHub Issue 实现 |
| 量化指标 | 8 维总分 | val loss | 5 维加权评分（达标线 9.0） |
| 人的参与 | 每轮暂停等人确认 | 全自主 | 循环外调优 program.md |
| 保护机制 | git revert 硬保护 | git revert 硬保护 | 交叉审核软保护（无回退） |

## 关联

- 经验沉淀续作：[[auto-optimize-skill]]
- 质量保证路线：属 [[硬性保护与软性保护]] 的硬保护阵营
- Skill 自优化领域：与 [[skill迭代闭环]]、[[skill评测三原则]]、[[skill-for-skill元技能自举]] 构成同一问题域的独立印证
