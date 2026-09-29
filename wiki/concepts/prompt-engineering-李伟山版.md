---
type: concept
title: Prompt Engineering（李伟山版）
tags: [prompt-engineering, ai-engineering, 第一次进化]
related: [工程三次进化框架, context-engineering-李伟山版, harness-engineering-李伟山版]
created: 2026-07-20
updated: 2026-07-20
sources: ["[202605201731]干货从PromptContext到Harness工程的三次进化与终局之战原创.html"]
---
# Prompt Engineering（李伟山版）

Prompt Engineering 是 [[李伟山]] 提出的 [[工程三次进化框架]] 中的**第一次进化**。其本质被定义为"加约束的过程"——研究如何通过精心设计的输入最大限度激发模型的正确能力。

## LLM 续写本质

文章指出 LLM 底层逻辑为"极其擅长续写的系统"，预测"最有可能出现的内容"，但"最有可能"≠"真正想要"。输出质量由约束条件的具体化程度决定（以道歉信三层约束递进为例：无约束→基础约束→精细化约束，约束越具体输出越可用）。

## 五种核心技术

| 技术 | 说明 | 适用场景 |
|------|------|---------|
| Zero-shot | 直接指令 | 简单任务 |
| Few-shot | 给输入-输出范例 | 效果远好于零样本 |
| CoT（Chain-of-Thought） | 引导逐步推理 | 数学/逻辑任务效果显著 |
| Role Prompting | 赋予身份（如"20年经验Java架构师"） | 提升专业性 |
| Prompt Chaining | 复杂任务拆分为流水线串联 | 多步骤任务 |

## 边际衰减规律

随模型智能化提升（GPT-3 → GPT-4 / [[claude-3-与-claude-3-5|Claude 3]]），精心设计 Prompt 的边际效益显著降低。GPT-3 时代需精心 Few-shot 才能完成复杂任务，GPT-4 / Claude 3 时代随便一句话即可理解意图。模型语言理解能力的增强使写好 Prompt 的边际收益递减，真正瓶颈转移到上下文缺失，引出第二次进化 [[context-engineering-李伟山版]]。

这一衰减规律与 [[harness衰变定律]]（模型能力与 Harness 复杂度呈反比）形成跨阶段呼应。