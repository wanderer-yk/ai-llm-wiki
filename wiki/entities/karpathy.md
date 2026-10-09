---
type: entity
title: Andrej Karpathy
tags: [karpathy, autoresearch, ai研究自动化, 方法论源头]
related: [autoresearch, smallnest-autoresearch, val-loss改善才commit, autoresearch三原则, autoresearch软件开发迁移]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# Andrej Karpathy

Andrej Karpathy 是 autoresearch 自动研究工具的发布者，也是"把优化循环本身交给 AI 自主运行"这一方法论的源头人物。（独立可查证背景，非本文内容：Karpathy 是知名 AI 研究者与工程师，OpenAI 创始成员、曾任 Tesla AI 总监。）

## 本文中的 Karpathy autoresearch

据本文（2026-04-20）记载：Karpathy 于 2026 年 3 月发布 autoresearch，几天内 GitHub 收获 **5 万+ 星标**，介绍视频播放 **860 万次**，为约 **600 行**的开源 Python 工具（详见 [[autoresearch]]）。

其核心思想："把 AI 研究本身也交给 AI 来自主完成"。给 Agent 一个真实小型 LLM 训练环境（单 GPU、5 分钟训练预算），由 Agent 自主修改 `train.py`、跑实验、检查结果——**只有 val loss 改善才 commit，否则 git revert 回滚**；人类只需维护一份 `program.md`（相当于给 Agent 的"研究章程"）。预计每小时约 12 次实验，一夜可收获上百轮自动优化。

## 方法论定位

Karpathy 方案的精髓被本文提炼为 [[autoresearch三原则]]：量化目标、自主循环、只保留改进。其质量保证路线属 [[硬性保护与软性保护]] 中的**硬性保护**阵营（git revert 硬回退），与 [[smallnest-autoresearch]] 的交叉审核软保护形成有意的设计分歧。其"只保留可测量的改进，其余全部回滚"的核心循环被本文列为第一设计灵感，也是 [[达尔文skill]]（花叔）等平行迁移实践的共同源头。

## 关联

- [[val-loss改善才commit]] —— 其单一指标门控机制
- [[autoresearch软件开发迁移]] —— 本文对其方法论的软件开发迁移
- [[smallnest-autoresearch]] —— 迁移落地项目
