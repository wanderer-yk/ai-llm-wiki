---
type: concept
title: Pre-PR预审机制
tags: [code-review, ai-coding, 流程, 质量]
related: [gao-jie-mo-xing-shen-cha-di-jie-mo-xing, kua-chang-shang-mo-xing-dui-kang, safety-iron-rule, ai-you-hao-yan-fa-gui-fan]
created: 2026-06-08
updated: 2026-06-08
sources: ["用Agent评测思路管理AICoding31万行代码AI重构的实践.html"]
---
# Pre-PR预审机制

AI Coding场景下解决Code Review瓶颈的关键流程创新。核心思路：**提交前RD必须先用AI多轮自查修复所有可发现问题，再提交标准PR文档，使Reviewer仅聚焦业务语义**。

## 为什么"不能省"

AI Coding使编码时间被极大压缩，压力系统性向下游CR环节集中，形成新的瓶颈（木桶效应）。如果CR效率不提升，AI Coding的提效红利会被CR瓶颈吞掉。

## 流程

1. RD完成代码编写
2. **AI多轮自查**——修复所有可发现的代码质量问题
3. AI按模板生成标准PR文档
4. 提交PR进入正式Review流程
5. Reviewer仅聚焦业务语义审查

## 人工CR的价值转变

从"你写得对吗？"转变为"**我们是否在正确的约束下解决正确的问题？**"

## AI辅助CR的两种模式

1. [[gao-jie-mo-xing-shen-cha-di-jie-mo-xing|高阶模型审查低阶模型]]：用高配模型作为Judge Model审查低阶模型的编码产出
2. [[kua-chang-shang-mo-xing-dui-kang|跨厂商模型对抗审核]]：不同厂商模型互相审查对方编码，通过能力差异化形成互补

## 关联

- 与[[safety-iron-rule|安全铁律]]共享"前置校验"的工程哲学
- 是[[ren-ren-dui-qi-ren-ji-dui-qi|人人对齐→人机对齐]]方法论在CR环节的具体落地