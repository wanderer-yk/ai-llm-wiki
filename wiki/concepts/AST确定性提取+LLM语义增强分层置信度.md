---
type: concept
title: AST 确定性提取+LLM 语义增强分层置信度
tags: [置信度, AST, LLM语义增强, 图谱质量, INFERRED]
related: [umodel, UModel六阶段构建流水线, 个人Wiki到代码Wiki同范式不同确定性, ast加符号表联合分析, Entity+Log+Link三元组建模]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱.html"]
---
# AST 确定性提取+LLM 语义增强分层置信度

"AST 确定性提取 + LLM 语义增强分层置信度"是 [[umodel|UModel]] 代码知识图谱的质量控制核心机制：**结构关系由 AST 解析确定性提取（置信度 1.0），语义信息由 LLM 增强补充（置信度 0.6-0.9）并标注 `INFERRED` 供 Agent 选择性采信**。两条轨道分层标注，Agent 按场景选择信任阈值。

## 原文流程对比（verbatim）

```text
个人 Wiki：   原始资料 → [LLM 抽取] → 对齐 → UModel → Wiki 页面
                        ↑ 全程依赖 LLM，置信度 0.4-0.9
代码 Wiki：   代码仓库 → [AST 确定性提取] + [LLM 语义增强] → UModel → CLI 查询
                        ↑ 结构关系确定（1.0）   ↑ 摘要/归属补充（0.6-0.9）
```

## 标注机制

```text
Entity ID = md5(repo_id:pk_value)
__confidence__                # 每条关系标注
__extraction_method__         # EXTRACTED / INFERRED / AMBIGUOUS
```

LLM 轨道产出（模块摘要、文档-代码关联、组件归属）标注 `INFERRED`；Agent 场景化信任阈值：RCA 优先高置信度关系，探索场景可放宽。

## 确定性的价值论证：RCA 推理链信任前提

文章的核心推理：calls 关系若是 LLM 猜的，整条 RCA 推理链就不可靠；AST 关系可无条件信任。作者以"张城和元乙是不是同一个人"自证 LLM 抽取不确定性——LLM 无法从文本确定双署名是否同一实体（0.4-0.9），但 `import pkg/a2a`、`func (s *Server) HandleRequest()` 等结构关系是确定的（1.0）。

## 与相关概念的对照

- 有赞 [[ast加符号表联合分析]]：同为确定性结构提取路线（AST 提供程序骨架 + 符合表记录标识符），但未引入置信度分层标注机制；UModel 的 `__extraction_method__` 三值标注是其增量设计。
- RESOLVE 阶段（[[UModel六阶段构建流水线]]）确保跨文件引用解析同样不依赖 LLM。