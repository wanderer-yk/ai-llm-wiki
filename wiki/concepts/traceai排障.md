---
type: concept
title: TraceAI排障
tags: [ai排障, 链路追踪, agent, 代码问答]
related: [code-insight, 代码问答, 调用关系图谱]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202601061754]AI实践CodeInsight代码搜索定位的实践分享.html"]
---
# TraceAI排障

TraceAI 是 [[code-insight|Code Insight]] 系统的三大落地场景之一，实现 Agent 自主结合代码问答能力与链路追踪日志进行线上问题定位。

## 工作机制

1. 接收线上问题告警/链路追踪日志
2. Agent 自主分析异常信息，识别可疑代码区域
3. 调用[[代码问答]]能力定位相关代码实现
4. 结合[[调用关系图谱]]追踪调用链路
5. 生成问题定位报告

## 核心价值

- 将代码问答能力与运维可观测性（链路追踪）结合
- Agent 自主完成排障流程，减少人工介入
- 从"人看日志→人搜代码→人分析"升级为"Agent 看日志→Agent 搜代码→Agent 分析"

## 待深入问题

- Agent 的具体工具调用链路和规划策略细节未详细展开
- TraceAI Agent 落地仍为未来方向之一
