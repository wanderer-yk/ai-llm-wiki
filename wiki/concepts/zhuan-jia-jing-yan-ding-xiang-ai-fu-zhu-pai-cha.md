---
type: concept
title: 专家经验定向+AI辅助排查
tags: [技术债, ai辅助, 专家经验, 排查]
related: [ji-shu-zhai, jing-yan-jia-zhi-zhuang-yi, agent-ping-ce-xi-tong]
created: 2026-06-08
updated: 2026-06-08
sources: ["用Agent评测思路管理AICoding31万行代码AI重构的实践.html"]
---
# 专家经验定向+AI辅助排查

[[ji-shu-zhai|技术债]]梳理的人机协作方法：核心开发圈定高危边界（定向），AI做穷举扫描（排查）。

## 方法论

1. **专家经验定向**——核心开发者凭借经验圈定哪些模块/路径最可能有隐患
2. **AI辅助排查**——AI在圈定范围内做穷举式扫描，发现人眼难以发现的隐藏问题

## 实践成果

- AI短时间内帮助工程师定位了**10个隐藏极深、靠肉眼极难发现**的性能隐患
- 完成3个P0 + 2个P1技术债梳理

## 与经验价值转移的关系

这一方法是[[jing-yan-jia-zhi-zhuang-yi|经验价值转移]]的具体体现：AI已可替代"能看全"的能力，但"能判断什么重要"（圈定高危边界）仍需人主导。