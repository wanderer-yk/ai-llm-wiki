---
type: concept
title: Deep Sleep 六维评分与晋升门控
tags: [openclaw, 长期记忆, 评分, 门控]
related: [Dreaming三阶段演进, light-sleep摄取与去重, REM睡眠主题反射与候选真理, 记忆召回与反馈环, 验证门禁化]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604151800]OpenClaw长期记忆优秀管线与玄学效果.html"]
---
# Deep Sleep 六维评分与晋升门控

Deep Sleep 是 [[openclaw]] Dreaming 系统的第三阶段：对经 Light/REM 筛选的记忆候选按六个信号维度加权评分，通过三重晋升门控后追加到 `MEMORY.md`。评分与门控均为机械公式，无 LLM 参与。

## 六维评分表（原文）

| 信号 | 权重 | 计算方式 | 含义 |
|------|------|----------|------|
| 频率 (Frequency) | 0.24 | `min(1, ln(signalCount + 1) / ln(11))` | 被回忆的总次数（recall + daily + grounded） |
| 相关性 (Relevance) | 0.30 | `totalScore / max(1, signalCount)` | 每次被检索时的平均质量分 |
| 多样性 (Diversity) | 0.15 | `min(1, max(uniqueQueries, recallDays) / 5)` | 不同查询/日期上下文的覆盖宽度 |
| 时效性 (Recency) | 0.15 | `exp(-λ × ageDays)`，λ = ln(2)/14 | 指数衰减，半衰期 14 天 |
| 巩固度 (Consolidation) | 0.10 | `max(0.55×spacing+0.45×span, groundedCount/3)` | 多日重现 或 grounded 信号强度 |
| 概念丰富度 (Conceptual) | 0.06 | `min(1, conceptTags.length / 6)` | Concept 标签密度 |

巩固度双分支公式：

```
// 分支 1：基于 recallDays 的时间跨度
spacing = min(1, ln(recallDays.length - 1) / ln(4))
span    = min(1, (maxDay - minDay) / 7天)
consolidation_a = 0.55 × spacing + 0.45 × span

// 分支 2：基于 grounded 信号计数
consolidation_b = min(1, groundedCount / 3)

consolidation = max(consolidation_a, consolidation_b)
```

阶段信号加权提升与最终分数：

```javascript
phaseBoost = LIGHT_BOOST_MAX(0.06) × lightStrength × lightRecency
           + REM_BOOST_MAX(0.09) × remStrength × remRecency
// 衰减半衰期同为 14 天
```

```ini
score = Σ(weight_i × component_i) + phaseBoost
```

## 晋升门控三条件

1. `score ≥ 0.80`（Dreaming 配置默认最低综合分）
2. `totalSignalCount ≥ 3`（recallCount + dailyCount + groundedCount ≥ 3）
3. `max(uniqueQueries, recallDays.length) ≥ 3`

## 晋升后行为：重新水合

通过门控的候选先**重新水合**——从实时日文件重读片段内容，防止写入过时/已删除内容——再追加到 `MEMORY.md`。

## 分析

- 门控设计 ↔ [[验证门禁化]]：硬性条件不满足即不晋升的跨场景同构。
- 语义盲区：六维统计评分无 LLM 语义判断，"花生过敏"类低频重要信息可能因信号不足永远无法晋升——[[RDSClaw]] 以 LLM CRUD + Evergreen 补强此环节。
- 正反馈环：评分消费召回信号（[[记忆召回与反馈环]]），"越被检索 → 越容易晋升"。
