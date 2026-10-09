---
type: source
title: 如何用 UModel 构建一个会成长的个人 Wiki
tags: [UModel, 个人Wiki, 知识管理, 待补全]
related: [umodel, 张城, 个人Wiki到代码Wiki同范式不同确定性, knowledge-wiki]
created: 2026-10-09
updated: 2026-10-09
authors: [张城]
year: 
url: ""
venue: ""
sources: ["[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱.html"]
---
# 如何用 UModel 构建一个会成长的个人 Wiki

[[张城]]（元乙）的前作，在《[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱》中被引用，是个人 Wiki 建模层范式的来源：以 Set/Link/Field 原语组织个人知识，流程为"原始资料 → [LLM 抽取] → 对齐 → UModel → Wiki 页面"。

**注意：本页元数据不完整**——来源文章未给出该前作的发表渠道、日期与 URL，本页为占位记录，待获取原始出处后补全 frontmatter 与正文。

## 核心流程（据后作转述）

```text
原始资料 → [LLM 抽取] → 对齐 → UModel → Wiki 页面
           ↑ 全程依赖 LLM，置信度 0.4-0.9
```

## 与代码 Wiki 的范式关系

后作提出 [[个人Wiki到代码Wiki同范式不同确定性]]：同一套 UModel 建模范式，个人 Wiki 全程依赖 LLM 抽取（置信度 0.4-0.9），而代码 Wiki 引入"模型层的确定性保证"——AST 确定性提取（1.0）+ LLM 语义增强（0.6-0.9，标注 `INFERRED`）。详见 [[AST确定性提取+LLM语义增强分层置信度]]。

## 与本 Wiki 项目自身的关联

该前作与本 Wiki 项目的定位（结构化知识层辅助人机协作）直接相关，其"会成长的 Wiki"理念与有赞 [[knowledge-wiki]]、腾讯 [[活文档机制]] 等知识管理实践同属一个主题簇。