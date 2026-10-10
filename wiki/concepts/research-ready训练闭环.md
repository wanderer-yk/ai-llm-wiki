---
type: concept
title: Research-Ready 训练闭环
tags: [rl训练, 自动化, hermes-agent, 训练框架]
related: [hermes-agent, 内外双路径自进化, 批量数据生成, opd机制, rl-cli标准化训练四阶段, agent轨迹, karpathy, autoresearchkarpathy-原版, 验证门禁化]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践.html"]
---
# Research-Ready 训练闭环

Research-Ready 训练闭环是 [[hermes-agent]] 内置的自动化 RL 训练框架（README.md 官方表述 "Research-Ready"），覆盖“**数据合成 → 质量筛选 → RL 环境构建 → 小规模实验 → 正式训练 → 自动评估**”的完整流程。命名上强调“完整闭环”而非单纯模型训练。

## 流程与组件映射

| 阶段 | 组件 |
|---|---|
| 数据合成 | [[批量数据生成]]（`batch_runner.py` / `mini_swe_runner.py`）、[[opd机制]] |
| 数据格式与压缩 | [[sharegpt格式]]、[[轨迹头尾保护压缩]] |
| RL 环境与训练 | [[rl-cli标准化训练四阶段]]（`rl_cli.py`）、[[grpo算法]] |
| 奖励设计 | [[多维度组合奖励]]、[[奖励函数设计黄金法则]]、[[toolcontext真实验证]] |

## 与 AutoResearch 的对照

来源文章将 Hermes RL 闭环与 Karpathy 的 AutoResearch（引文 [3]，https://github.com/karpathy/autoresearch ，见 [[karpathy]]、[[autoresearchkarpathy-原版]]）对比，称二者理念类似（自动化 RL 训练，可单 GPU 运行），但 Hermes “更加完善和成熟”。

## 工程化特征

- 强制 `rl_test_inference` 作为正式训练前的防错防线，与 [[验证门禁化]] 的“硬性门控”理念一致。
- 训练异步进行，`rl_check_status` 建议至少间隔 30 分钟。
- 教师模型示范数据采用旗舰模型（默认 `anthropic/claude-opus-4.6`）立 Baseline，小步快跑渐进训练。