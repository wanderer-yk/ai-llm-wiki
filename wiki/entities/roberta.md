---
type: entity
title: RoBERTa
tags: [模型, NLP, 意图分类, 小模型]
related: [small-model-beats-llm-in-classification, intent-planning, bert]
created: 2026-06-08
updated: 2026-06-08
sources: ["颠覆传统意图规划上下文工程数据自迭代让企业智能办公助手效能跃升200V10.html"]
---
# RoBERTa

110M参数的BERT类预训练语言模型，在[[fucheng|富城]]的文章中被指定为企业意图分类任务的首选模型。

## 核心优势

文章提出，在精确分类任务上，微调RoBERTa比微调7B LLM具有三大优势：

1. **准确率更高**：微调后可达95%+，而LLM少样本分类难以突破90%
2. **推理更快**：110M参数量远小于7B模型
3. **成本更低**：训练和推理的资源消耗显著低于大模型

## 在企业场景的定位

RoBERTa是企业场景[[small-model-beats-llm-in-classification|小模型碾压LLM]]主张的核心支撑。文章认为企业场景意图封闭且有限，适合小模型做意图识别，而LLM+提示词在意图超过10个时，零样本/少样本分类准确率很难突破90%，低于[[intent-recognition-95-baseline|95%交付线]]。

## 与[[bert|BERT]]的关系

RoBERTa是BERT的改进变体，文章将其作为BERT类小模型的代表进行推荐。