---
type: concept
title: interview 机制
tags: [qoder, 需求澄清, 意图理解, spec-driven]
related: [动态spec机制, spec-driven起手范式, qoder, ai-coding第一性原理]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202601242318]QoderQuest10把执行交给AI把选择留给人类.html"]
---
# interview 机制

interview 机制是 [[qoder]] Quest 模式的核心动态机制之一，用于在用户意图模糊时主动提问澄清需求，帮助用户从模糊意图走向精确规格。

## 工作方式

当用户输入一个模糊的需求描述时，Quest 模式不会直接开始编码，而是通过 interview 阶段主动提出针对性问题：

1. **识别模糊点** — 分析用户描述中缺失的关键信息
2. **主动提问** — 以结构化方式向用户确认需求细节
3. **收敛意图** — 通过多轮交互逐步将模糊需求转化为可执行的规格

## 与 spec 的关系

interview 和 [[动态spec机制]] 是 Quest 模式前后衔接的两个核心阶段：

- **interview** — 澄清意图（输入端的质量保证）
- **spec** — 生成规格（执行依据的确定化）

完成 interview 和 spec 后，Quest 模式比 Editor 模式更少需要人类介入，表现出更强的自主编程能力。

## 设计理念

interview 机制体现了 [[ai-coding第一性原理]] 中"价值"和"优先级"维度的前置——在编码之前确保做的事情是对的。这与 [[研发范式前移]] 的理念一致：将问题解决的关键环节从编码调试期前移至需求设计期。

## 跨领域关联

- 与 Specflow 的 [[blocker-gate]] 类似——都是在编码前强制对齐需求
- 与马上消费的 [[意图规划]] 中的 Query 改写互补——前者用于编码场景的需求澄清，后者用于企业助手场景的意图理解
- 与 [[harness-engineering]] 的"任务入口"要素呼应——确保输入质量