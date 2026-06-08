---
type: concept
title: 数据自迭代
tags: [方法论, 质检, 数据驱动, 持续优化]
related: [intent-planning, context-engineering, quality-inspection-ai, enterprise-intelligent-assistant]
created: 2026-06-08
updated: 2026-06-08
sources: ["颠覆传统意图规划上下文工程数据自迭代让企业智能办公助手效能跃升200V10.html"]
---
# 数据自迭代

[[enterprise-intelligent-assistant|企业智能办公助手]]三层方法论架构的第三层，目标是**驱动持续优化**。

## 数据迭代闭环

完整流程：**日志记录 → 弱样本抓取 → 标注回流 → 模型重训 → 动态部署**

文章强调意图识别项目没有完成态，只有持续迭代态，约需3个月趋于稳定。

## 质检机制

核心组件为[[quality-inspection-ai|质检AI]]，基于通用LLM的语义理解进行自动化质量评估。

- 混淆矩阵一致性基准线：80%
- 实测一致性：89%
- 提示词需经历数轮迭代

## 核心价值

- 产品迭代从**主观反馈**转为**数据驱动**
- 新知识上线成本降低**一个数量级**
- 建立常态化质检机制，避免产品迭代失控

## 准确率容忍梯度

数据自迭代环节的质检AI对准确率的容忍度与意图规划环节形成梯度：
- 意图分类：95%（严格）
- 质检判断：80%基准/89%实测（中等）
- [[nlp2sql-limitation|NLP2SQL]]：<80%（不可接受）