---
type: concept
title: 高阶模型审查低阶模型
tags: [code-review, ai, 模型, 质量]
related: [kua-chang-shang-mo-xing-dui-kang, pre-pr-ji-zhi, quality-inspection-ai]
created: 2026-06-08
updated: 2026-06-08
sources: ["用Agent评测思路管理AICoding31万行代码AI重构的实践.html"]
---
# 高阶模型审查低阶模型

用高配置模型作为Judge Model审查低阶模型的编码产出，是AI辅助Code Review的一种模式。

## 原理

不同能力的模型在不同维度上有差异化表现。高阶模型具备更强的全局理解力和业务语义推理能力，适合作为"裁判"审查低阶模型的编码细节。

## 与跨厂商模型对抗审核的关系

[[kua-chang-shang-mo-xing-dui-kang|跨厂商模型对抗审核]]是本模式的扩展——不同厂商模型互相审查对方编码，通过能力差异化形成互补，实测CR覆盖面更全。

## 局限性

文章未提及具体模型选型（哪些是"高阶"、哪些是"低阶"），也未给出审查效果的量化数据。