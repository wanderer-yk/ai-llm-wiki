---
type: entity
title: Ragas
tags: [RAG, 评测工具, LLM-as-a-Judge, 测试集生成, 开源工具]
related: [rag全链路, ragas评估指标, 评测集生成, ragas查询类型分类, ragas知识图谱构建, 场景生成, 场景采样两阶段解耦, llm-as-judge, 评测agent乐观偏差, agent评测四要素, 评测集优先于知识库]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202605181736]RAG全链路技术详解.html"]
---

# Ragas

Ragas（Retrieval Augmented Generation Assessment）是一款用于 RAG（Retrieval Augmented Generation）框架效果评估的自动化工具。据本文表述，「Ragas 是一款广受欢迎的用于 RAG 框架效果评估的自动化工具」，官方文档见 `https://docs.ragas.io/en/latest/getstarted/`（补充说明：Ragas 为社区开源的 RAG 评测框架，此为可独立验证的公开背景，非本文主张）。

## 核心理念与双维度评估

- **LLM-as-a-Judge**：「Ragas 的核心理念是使用 LLM 来评估 LLM（LLM-as-a-Judge），通过一系列自动化的指标来衡量 RAG 系统的性能」，对应 [[llm-as-judge]]。
- **双维度拆解**：Ragas 将 RAG 的评估拆解为检索（Retrieval）和生成（Generation）两个维度，与 contexts/answer 维度映射一致。
- 文章声称「实验显示其指标结果的准确性已经非常接近人工评测结果」——该声明无实验口径（样本量/任务类型），与 [[评测agent乐观偏差]] 构成跨源张力，已纳入 [[ai工程量化效果声明追踪]]。

## 五指标体系

Context Precision、Context Recall（检索侧）与 Faithfulness、Answer Relevancy、Noise Sensitivity（生成侧），输入签名谱系 2→3→4 参，详见 [[ragas评估指标]]。

## 测试集自动生成能力

Ragas 除评估外还提供测试集自动生成工具：查询类型 2×2 矩阵（[[ragas查询类型分类]]）→ 内部知识图谱构建（[[ragas知识图谱构建]]，Document Splitter → Extractors → Relationship builder）→ 场景生成（[[场景生成]]，节点×查询长度×查询风格×人设四参数）→ Query Synthesizer 实操 → reference 生成。场景规划与样本生成在代码层解耦为两阶段（[[场景采样两阶段解耦]]），LLM 在 `_generate_sample` 中同时产出问题与标准答案，输出 `SingleTurnSample(user_input, reference_contexs, reference)`。

## 内置组件与 API 要素

| 组件/类型 | 说明 |
|---|---|
| Query Synthesizer | 查询合成器基类，模块路径 `ragas.testset.synthesizers.base_query`，自定义需实现 `_generate_scenarios` 与 `_generate_sample` 两个异步方法 |
| NERExtractor | 基于规则的提取器示例 |
| Keyphrase Extractor | 关键词/关键短语提取器，支撑第四类关系构建（适用于无明确命名实体的纯技术文档） |
| Scenario | 场景方案对象（节点文本＋人设＋风格等） |
| SingleTurnSample | 单轮测试样本输出（user_input、reference_contexs、reference） |
| TestsetSample | 一道完整题目 |

## 待核事项

- `reference_contexs`（疑为 `reference_contexts`）是源文笔误还是实际 API 拼写，待对照 Ragas 文档核实。
- `EntityQuerySynthesizer` 是官方内置类还是教学示例（文中代码为未完成的教学示意）。
- 全自动建边→出题→出答案的自举链可信度与 [[评测agent乐观偏差]]、[[评测集优先于知识库]] 的张力待整合裁决。