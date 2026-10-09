---
type: entity
title: AutoResearch（Karpathy 原版）
tags: [autoresearch, karpathy, ai研究自动化, 单agent循环]
related: [karpathy, smallnest-autoresearch, val-loss改善才commit, autoresearch三原则, program-md规则核心, 达尔文skill]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# AutoResearch（Karpathy 原版）

AutoResearch 是 Andrej Karpathy 于 2026 年 3 月发布的开源 Python 工具（约 600 行），把 AI 研究本身交给 AI 自主完成：Agent 在真实小型 LLM 训练环境中自主修改 `train.py`、跑实验、检查结果，只保留可测量的改进。注意与鸟窝迁移后的软件开发项目 [[smallnest-autoresearch]] 区分。

## 核心机制

- **运行环境**：单 GPU、5 分钟训练预算，每小时约 12 次实验，一夜可收获上百轮自动优化
- **实验对象**：`train.py`（Agent 自主修改）
- **门控机制**：[[val-loss改善才commit]] —— 只有 val loss 改善才 commit，否则 `git revert` 回滚，"绝不将就"
- **人类契约**：`program.md`（"研究章程"），定义目标与约束，人类不逐轮介入
- **谱系定位**：属"单 Agent 自循环"路线——与 Ralph Wiggum 方法（见 [[ralph-wiggum方法]]）同用单 Agent，但以量化指标（val loss）取代盲循环，核心创新是量化"什么是改进"

## 三原则

[[autoresearch三原则]]：① 量化目标（val loss 是唯一判断标准）；② 自主循环（无需人类每轮介入）；③ 只保留改进（退化就回滚）。

## 影响与迁移

该项目发布后数天即获 5 万+ 星标、视频播放 860 万次，催生了至少两条平行迁移实践：[[smallnest-autoresearch]]（软件开发领域，本文核心）与 [[达尔文skill]]（Skill 优化领域，花叔出品）。三者共用三原则，但量化指标（val loss / 5 维评分 / 8 维总分）与人的参与程度（全自主 / 循环外调优 / 每轮暂停确认）各不相同，详见 [[autoresearch三项目对比]]。
