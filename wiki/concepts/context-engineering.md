---
type: concept
title: 上下文工程
tags: [方法论, 知识管理, 企业AI]
related: [intent-planning, data-self-iteration, entity-merge-wide-table, faq-conversion, agent-collaboration-delivery, conflict-resolution-four-principles]
created: 2026-06-08
updated: 2026-06-08
sources: ["颠覆传统意图规划上下文工程数据自迭代让企业智能办公助手效能跃升200V10.html"]
---
# 上下文工程

[[enterprise-intelligent-assistant|企业智能办公助手]]三层方法论架构的第二层，目标是**保障信息准确交付**。

## 三大模块

### 1. 知识入库
让知识"活"下来，解决四大难点：

| 难点 | 解决方案 |
|------|----------|
| 动态文档更新 | 空间监测+增量更新、版本管理+覆盖更新 |
| 多模态注入 | ASR→AI生成议程→结构化入库→议程为最小召回单元 |
| 客服知识沉淀 | 人工工单自动归纳→二次拦截→自动归纳后转人工 |
| 信源冲突 | [[conflict-resolution-four-principles\|四原则裁决]] |

### 2. 标准数据注入查询
否定[[nlp2sql-limitation|NLP2SQL]]（<80%准确率不可接受），提出两条务实路径：

- [[entity-merge-wide-table|主体合并]]：围绕同一主体预聚合多系统数据为大宽表，准确率~100%
- [[faq-conversion|FAQ转化]]：将动态数据转化为静态知识对，准确率98%+

### 3. Agent协同调用
办理类任务的必选路径，知识/数据注入无法覆盖。详见 [[agent-collaboration-delivery|Agent协同调用交付]]。

## 渠道优先级（信源冲突裁决四原则）

版本优先 → 时间优先 → 渠道优先 → 结合个人信息

其中渠道优先级为：**人工FAQ > 内部文档 > 外部搜索 > 模型世界知识**

这进一步强化了"通用大模型世界通识在企业场景是灾难"的主张。