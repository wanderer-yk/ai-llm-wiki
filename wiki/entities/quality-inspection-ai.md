---
type: entity
title: 质检AI
tags: [模块, 质检, LLM应用]
related: [data-self-iteration, enterprise-intelligent-assistant]
created: 2026-06-08
updated: 2026-06-08
sources: ["颠覆传统意图规划上下文工程数据自迭代让企业智能办公助手效能跃升200V10.html"]
---
# 质检AI

基于通用大模型的回答内容质量检验模块，是[[data-self-iteration|数据自迭代]]方法论的核心组件。

## 工作原理

利用通用LLM的语义理解能力，对[[enterprise-intelligent-assistant|企业智能办公助手]]的回答进行自动化质量评估，替代传统的人工抽检机制。

## 核心指标

- **混淆矩阵一致性**：质检AI与人类判断的一致性
- **基准线**：80%
- **实测值**：89%
- 作者补充"真实数值可能更高，因为有些问题真人也难判断"

## 关键结论

- 通用大模型可用于质检场景
- 提示词需经历数轮迭代才能达到可用水平
- 95%的低质量回答属于四类：拒绝回答、答非所问、部分回答、回答绕弯弯；数值类错误不到1%

## 准确率容忍梯度

质检AI的89%一致性与意图识别的[[intent-recognition-95-baseline|95%否决线]]形成对比，体现了文档内部不同环节对准确率容忍度的梯度设计：
- 意图分类：95%严格
- 质检判断：80%基准，89%实测
- [[nlp2sql-limitation|NLP2SQL]]：<80%不可接受