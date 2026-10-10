---
type: concept
title: light-sleep 摄取与去重
tags: [openclaw, 长期记忆, 去重]
related: [Dreaming三阶段演进, REM睡眠主题反射与候选真理, Deep-Sleep六维评分与晋升门控, 记忆写入双路径]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604151800]OpenClaw长期记忆优秀管线与玄学效果.html"]
---
# light-sleep 摄取与去重

Light Sleep（浅睡眠）是 [[openclaw]] Dreaming 系统的第一阶段：从三类信号源机械摄取候选记忆片段并去重。**该阶段完全不调用 LLM**，摄取与去重是纯确定性文本处理，无法理解语义近似、只能依赖字面重叠度判断重复。

## 摄取规格（原文）

| 信号源 | 规格 |
|---|---|
| 日记忆文件 `memory/YYYY-MM-DD.md` | 逐行提取候选片段：最小 8 字符，最大 280 字符，最多 4 行合并为一个块 |
| 会话转录 | 按 Agent 和 Session 聚合的历史消息：每次最多扫描 240 条，每个文件 12~80 条 |
| 短期回忆存储 | `memory/.dreams/short-term-recall.json` 中已有记录 |

## 去重规则

- Jaccard 相似度阈值 **0.9**；重复项合并时取最高的 recallCount、maxScore，合并 queryHashes 和 recallDays。
- 输出：写 `## Light Sleep` 块 + 为每个候选记录 `lightHits` 计数（供 Deep Sleep 加权）。

## 分析

- Light Sleep 的确定性机械规则与写入/默认晋升环节的 LLM 弱约束形成对比——管线是"机械规则与 LLM 决策混合"的架构。
- 字面重叠去重的语义盲区：语义近似但字面不同的同事实会逃过去重（REM 阶段将阈值提高到 0.88 也仍是字面度量）；[[RDSClaw]] 以向量相似度 + LLM CRUD 补强。
