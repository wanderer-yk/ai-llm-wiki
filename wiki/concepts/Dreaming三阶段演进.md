---
type: concept
title: Dreaming 三阶段演进
tags: [openclaw, 长期记忆, 记忆巩固]
related: [openclaw, light-sleep摄取与去重, REM睡眠主题反射与候选真理, Deep-Sleep六维评分与晋升门控, 梦境日记叙事生成, memory-flush机制]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604151800]OpenClaw长期记忆优秀管线与玄学效果.html"]
---
# Dreaming 三阶段演进

Dreaming（梦境系统）是 [[openclaw]] 的后台记忆巩固系统：将短期信号逐步转化为长期记忆。**opt-in 功能**（`DEFAULT_MEMORY_DREAMING_ENABLED = false` 默认禁用），启用后自动创建 Cron 任务默认 `0 3 * * *`（每日凌晨 3 点全量扫描，时区依配置），每次扫描按 Light → REM → Deep 顺序执行。

## 三阶段概览

| 阶段 | 职责 | 确定性 | LLM 调用 |
|---|---|---|---|
| Light Sleep | 摄取与去重 | 纯确定性文本处理 | 无（明示） |
| REM Sleep | 主题反射 + 候选真理筛选 | 机械统计 | 无（日记除外） |
| Deep Sleep | 六维评分 + 晋升门控 | 机械公式 | 无（日记除外） |

三阶段各自的梦境日记（[[梦境日记叙事生成]]）是管线中唯一的 LLM 调用点，产出追加到 `DREAMS.md`，仅供人类阅读、不参与晋升评分。

## 关键定位

Dreaming 提供的是**确定性但有语义盲区与时效代价**的记忆晋升路径：Jaccard 去重无语义理解（同事实可能多版本）、六维统计评分无 LLM 语义判断（低频重要信息可能被埋没）、Cron 跨日信号积累延迟（时效信息失效）——管线确定性 ≠ 记忆正确性。"玄学效果"主要归因于写入与默认晋升路径，Dreaming 是确定性替代方案而非完整解药。

## 细节页

- [[light-sleep摄取与去重]]
- [[REM睡眠主题反射与候选真理]]
- [[Deep-Sleep六维评分与晋升门控]]
