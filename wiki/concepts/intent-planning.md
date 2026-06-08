---
type: concept
title: 意图规划
tags: [方法论, 意图理解, 企业AI]
related: [context-engineering, data-self-iteration, query-rewriting, small-model-beats-llm-in-classification, enterprise-intelligent-assistant]
created: 2026-06-08
updated: 2026-06-08
sources: ["颠覆传统意图规划上下文工程数据自迭代让企业智能办公助手效能跃升200V10.html"]
---
# 意图规划

[[enterprise-intelligent-assistant|企业智能办公助手]]三层方法论架构的第一层，目标是**精准理解用户意图**。

## 落地三步走

### 1. Query改写
解决用户输入残缺问题，包含三类动作：
- **多轮会话补全**：将省略的上下文信息补全
- **指代消解**：将"它"、"那个"等指代替换为具体实体
- **长文摘要**：对冗长输入进行提炼

详见 [[query-rewriting|Query改写]]。

### 2. 意图识别
核心主张：[[small-model-beats-llm-in-classification|微调小模型碾压LLM]]。

- 微调[[roberta|RoBERTa]]（110M参数）可达95%+准确率
- LLM少样本分类在意图超过10个时难以突破90%
- 95%为[[intent-recognition-95-baseline|一票否决线]]，低于此值项目不可交付
- 意图识别无完成态，需约3个月数据迭代周期趋于稳定

### 3. 意图挖掘
批判ToC产品的世界通识推荐问逻辑，提出**基于匿名会话关联的意图挖掘**：

- 利用历史匿名会话数据学习真实关联规律
- 确保每个意图有标准答案
- 规避通识推荐带来的知识盲区和可控性风险

## 核心洞察

企业场景意图封闭且有限，适合小模型做分类；ToC的通识推荐问逻辑在企业级场景是灾难。