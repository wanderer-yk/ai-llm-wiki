---
type: entity
title: AutoResearch 软件开发迁移
tags: [autoresearch, 方法论迁移, 自动化软件开发, 量化门控]
related: [karpathy, autoresearch, smallnest-autoresearch, autoresearch三原则, 5维度量化评分, 达尔文skill, 四阶段优化循环]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# AutoResearch 软件开发迁移

AutoResearch 软件开发迁移指把 Karpathy autoresearch 的"训模型实验循环"方法论映射为"软件开发任务循环"的范式：像 Karpathy 训模型一样开发软件——让 Agent 在量化指标门控下自主完成"领任务 → 实现 → 验证 → 只保留达标成果"的闭环。本文是其主要载体（落点为 [[smallnest-autoresearch]]）。

## 核心映射

| Karpathy 原版（ML 研究） | 迁移后（软件开发） |
|------|------|
| 修改 train.py | 实现 GitHub Issue |
| 跑 5 分钟实验 | 跑测试 |
| val loss 改善才保留（见 [[val-loss改善才commit]]） | 多维评分达标才合并（见 [[5维度量化评分]]） |

## 迁移中保持不变的部分

[[autoresearch三原则]]（量化目标、自主循环、只保留改进）跨领域成立：三方对比（ML 研究 / 软件开发 / Skill 优化）验证量化目标是共通核心，仅指标形态不同（val loss / 审核评分 / 8 维总分）。

## 迁移中必须改造的部分

1. **指标**：ML 有单一客观 metric（val loss），软件质量必须重构为多维加权评分
2. **质量保证**：原版以 git revert 硬保护兜底；软件开发侧本项目改用多 Agent 交叉审核软保护（见 [[硬性保护与软性保护]]），即[[人的参与程度反映领域特征]]所示——领域 metric 越主观，人的介入越多
3. **循环控制**：从开放实验循环变为 Issue 粒度的任务循环（[[四阶段优化循环]]），需补充 Issue 选择、权限边界与错误处理

## 平行迁移

同源方法的另一条迁移线是花叔的 [[达尔文skill]]（Skill 优化领域），说明该范式具备跨领域通用性，差异集中在量化指标设计与人的参与程度。
