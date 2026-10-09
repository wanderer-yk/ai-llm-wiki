---
type: concept
title: 个人 Wiki 到代码 Wiki 同范式不同确定性
tags: [范式迁移, 置信度, wiki, 确定性, UModel]
related: [umodel, 张城, sources/如何用UModel构建一个会成长的个人Wiki, AST确定性提取+LLM语义增强分层置信度, knowledge-wiki]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱.html"]
---
# 个人 Wiki 到代码 Wiki 同范式不同确定性

"个人 Wiki 到代码 Wiki 同范式不同确定性"是《[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱》提出的范式迁移论点：**同一套 UModel 建模范式（Set/Link/Field）从个人知识管理迁移到代码理解域时，引入了"模型层的确定性保证"——这是个人 Wiki 不具备的**。

## 两种流程的置信度对照

```text
个人 Wiki：   原始资料 → [LLM 抽取] → 对齐 → UModel → Wiki 页面
                        ↑ 全程依赖 LLM，置信度 0.4-0.9
代码 Wiki：   代码仓库 → [AST 确定性提取] + [LLM 语义增强] → UModel → CLI 查询
                        ↑ 结构关系确定（1.0）   ↑ 摘要/归属补充（0.6-0.9）
```

个人 Wiki 的知识源（文档、笔记）本身是自然语言，只能依赖 LLM 抽取，置信度 0.4-0.9；代码域的 `import pkg/a2a`、`func (s *Server) HandleRequest()` 等结构关系可经 AST 确定性解析，置信度 1.0，因此代码 Wiki 的关系层可无条件信任（详见 [[AST确定性提取+LLM语义增强分层置信度]]）。

## 概念缘起

作者 [[张城]] 前作《如何用 UModel 构建一个会成长的个人 Wiki》（见 [[sources/如何用UModel构建一个会成长的个人Wiki]]）建立了个人 Wiki 建模层范式；本文将同一范式迁移到代码域，并以确定性差异解释为何迁移后可信度跃升。

## 与本 Wiki 项目自身的关联

本 Wiki 项目本身即一个"面向人机协作的结构化知识层"，与该论点直接相关：如何为本 Wiki 的知识条目引入置信度/来源标注（对标 `__confidence__` + `__extraction_method__` 机制），与有赞 [[knowledge-wiki]] 的知识准入控制、[[评测集优先于知识库]] 的质量控制主张同属一个待探索问题簇。