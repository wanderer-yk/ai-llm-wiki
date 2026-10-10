---
type: concept
title: OPD 机制
tags: [rl训练, 数据合成, 蒸馏, hermes-agent, stub]
related: [hermes-agent, 批量数据生成, research-ready训练闭环, rl知识蒸馏降本论]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践.html"]
---
# OPD 机制

OPD（Hindsight-Guided On-Policy Distillation，后见引导的在策略蒸馏）是 [[hermes-agent]] 中最精细的 Teacher-Student 数据合成机制，学术出处为 Princeton 2026 年论文《OpenClaw-RL: Train Any Agent Simply by Talking》（来源引文 [4]，Y Wang, X Chen et al.，无链接，真实性待查），代码实现在 `environments/agentic_opd_env.py`。

**本页为 stub**：来源文章仅给出机制名称、论文出处与代码路径，未展开 OPD 的具体流程（如 hindsight 重标注方式、on-policy 采样细节、与 [[批量数据生成]] 的分工）。待补充证据后再扩展。

相关背景：RL 训练整体上被作者定位为知识蒸馏（大模型能力压缩到小模型），见 [[rl知识蒸馏降本论]]。