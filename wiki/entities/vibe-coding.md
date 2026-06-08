---
type: entity
title: Vibe Coding（氛围编码）
tags: [ai-coding, 开发模式]
related: [spec-driven-development, cursor, ai-bian-cheng-huan-jue]
created: 2026-06-08
updated: 2026-06-08
sources: ["治愈CursorAI编程的幻觉用它就够了.html"]
---
# Vibe Coding（氛围编码）

**Vibe Coding（氛围编码）** 是早期 AI Coding 的典型实践模式，通过简单对话、截图等方式快速生成页面代码。[[tian-ji-qian-duan-tuan-dui|天玑前端团队]] 在一年的实践中发现，这种模式在简单场景下效果良好，但在复杂中后台场景下暴露出根本性缺陷。

## 特征

- 以自然语言对话或截图直接驱动 AI 生成代码
- 缺乏标准化约束和前置规格定义
- 快速原型能力强，但输出质量不稳定

## 问题与瓶颈

在复杂场景下，Vibe Coding 导致：
- **碎片化输出**：AI 在缺乏明确约束时产生不一致的代码
- **上下文断层**：多轮对话中关键信息丢失或被稀释
- **需求共识缺失**：开发者和 AI 对需求理解存在偏差，且随迭代放大

## 演进方向

Vibe Coding 的瓶颈催生了向 [[spec-driven-development|规格驱动开发（SDD）]] 的转型——从"对话驱动"进化为"契约驱动"，从"氛围感知"进化为"规格约束"。[[specflow|Specflow]] 正是这一转型的产物。