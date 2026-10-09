---
type: concept
title: 提取校验修复三 skill 拆分
created: 2026-10-09
updated: 2026-10-09
tags: [skill, 校验, 角色分离, 流水线]
related: [ai自审偏差, checklist驱动skill, 多agent逆向工程初始化, skill稳定性决定论, 两阶段迁移流程]
sources: ["[202605201800]网盘存量代码迁移实战我们如何用三层架构管住AI的输出.html"]
---
# 提取校验修复三 skill 拆分

**提取校验修复三 skill 拆分**是本文的关键 Skill 设计决策：把提取和校验拆成独立 Skill，形成三段流水线。其动机是规避 [[ai自审偏差]]——同一个 Skill 里 AI"自己审查自己"天然放水，类比代码 Review 中作者自审的失效。

## 提取阶段三 Skill

| Skill | 职责 |
|-------|------|
| extractor | 以调用语句为入口扫描依赖，生成初始提取包 |
| validator | 独立校验，关注"还缺什么"而非"提了什么" |
| fixer | 只处理 validator 指出的缺口，定向补充 |

设计要点在 validator 的视角设定：它不检查"提了什么"，只检查"还缺什么"——用问题视角替代成果视角，降低自我确认倾向。

## 通用性佐证

该结构在转化阶段被复用为 **converter → validator → fixer**：同样的"生产者 → 独立校验者 → 定向修复者"角色组合，在不同任务类型上再次成立，说明拆分逻辑是通用的模式而非一次性技巧。

## 与既有概念的关系

与 [[多agent逆向工程初始化]]（Controller → Implementer → Reviewer 角色分离）同构：都通过角色/阶段分离实现视角隔离与故障分层定位。