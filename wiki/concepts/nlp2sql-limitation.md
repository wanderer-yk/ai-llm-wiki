---
type: concept
title: NLP2SQL局限性
tags: [技术否定, 准确率, 企业场景]
related: [context-engineering, entity-merge-wide-table, faq-conversion]
created: 2026-06-08
updated: 2026-06-08
sources: ["颠覆传统意图规划上下文工程数据自迭代让企业智能办公助手效能跃升200V10.html"]
---
# NLP2SQL局限性

[[fucheng|富城]]明确否定的技术路线。文章指出NLP2SQL在复杂企业业务场景下准确率难稳定突破**80%**，对容错率为零的企业场景等同于不可用。

## 否定理由

1. **准确率天花板**：<80%，远低于企业场景要求
2. **复杂性不可控**：企业数据库表结构复杂、关联关系深
3. **直接对接原系统API**：工作量巨大且脆弱

## 替代方案

文章提出两条务实路径替代NLP2SQL：

- [[entity-merge-wide-table|主体合并宽表]]：准确率~100%
- [[faq-conversion|FAQ转化]]：准确率98%+

## 与技术社区的对立

该否定与部分技术社区推崇NLP2SQL的做法形成对立。文章的核心立场是：企业场景应追求确定性工程替代概率性推理。