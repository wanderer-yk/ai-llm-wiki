---
type: concept
title: GRPO 算法
tags: [rl, 强化学习, 训练算法, hermes-agent, deepseek]
related: [deepseek, hermes-agent, 多维度组合奖励, 奖励函数设计黄金法则, toolcontext真实验证, rl-cli标准化训练四阶段, research-ready训练闭环, 内外双路径自进化]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践.html"]
---
# GRPO 算法

GRPO（Group Relative Policy Optimization）是由 [[deepseek]] R1 论文提出的强化学习算法：对同一问题生成 8~16 个回答，按奖励函数打分，让模型学习“多产出高分回答”。其关键优势是**无需单独训练 Reward Model**，直接使用规则化的奖励函数——来源文章作者自述“以前训练 Reward Model 煞费苦心却很难训练好”。

## Hermes 中的集成

Hermes 将 GRPO 训练流程封装为内置 Skill：`/skills/mlops/training/grpo-rl-training/SKILL.md`，其中包含[[奖励函数设计黄金法则]]；奖励设计示例见 `basic_grpo_training.py`（[[多维度组合奖励]]），并可通过 [[toolcontext真实验证]] 执行命令/读文件/联网/浏览器做真实验证。训练执行走 [[rl-cli标准化训练四阶段]]。

## 术语张力（需注意）

来源文章前段曾写“通过强化学习中的奖励机制（Reward Model）达到领域局部最优”，与 GRPO“免 Reward Model”的表述存在措辞混用：**奖励函数（规则化打分）≠ 习得的 Reward Model（单独训练的评分模型）**。引用本文相关表述时应加以区分。