---
type: concept
title: REM 睡眠主题反射与候选真理
tags: [openclaw, 长期记忆]
related: [Dreaming三阶段演进, light-sleep摄取与去重, Deep-Sleep六维评分与晋升门控]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604151800]OpenClaw长期记忆优秀管线与玄学效果.html"]
---
# REM 睡眠主题反射与候选真理

REM Sleep（快速眼动睡眠）是 [[openclaw]] Dreaming 系统的第二阶段：对 Light Sleep 输出的候选做主题反射与"候选真理"筛选，纯统计模式分析（机械规则，非 LLM 语义判断）。

## 主题反射

统计 concept tags（从文件路径和片段内容自动提取）出现频率计算主题强度，仅保留强度 ≥ `minPatternStrength` 的主题（默认值未披露）：

```
strength = min(1, (count / totalEntries) × 2)
```

## 候选真理置信度（四因子加权）

```python
confidence = avgScore × 0.45 + recallStrength × 0.25 + consolidation × 0.20 + conceptual × 0.10
其中：
  recallStrength = min(1, log1p(recallCount) / log1p(6))
  consolidation  = min(1, recallDays.length / 3)
  conceptual     = min(1, conceptTags.length / 6)
```

## 关键阈值

- 去重阈值提高到 **Jaccard 0.88**（比 Light Sleep 的 0.9 更严格，但仍为字面度量）；
- 仅保留置信度 ≥ **0.45**；
- 最多选 **3 条**候选真理。

## 输出

写 `## REM Sleep` 块 + 为每个候选记录 `remHits` 计数（供 Deep Sleep `phaseBoost` 加权，REM_BOOST_MAX = 0.09）。
