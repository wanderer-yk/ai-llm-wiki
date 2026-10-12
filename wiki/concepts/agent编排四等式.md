---
type: concept
title: agent编排四等式
tags: [编排, react, workflow, langgraph]
related: [react-agent, plan-and-execute模式, workflow优先于agent, harness与workflow主导权之辨, langchain, langchain语义标准化]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么-上.html", "[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么-下.html", "[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么.html"]
---
# agent编排四等式

[[韩春]] 对主流编排范式的代码等价还原（正文原表）：

| 框架概念 | 代码等价 | 语义 |
|---------|---------|------|
| ReAct | while + if | 循环执行：思考→行动→观察，直到完成 |
| Workflow | if + elif + else | 条件分支：根据任务类型选择不同处理链 |
| LangGraph | Workflow + StateNode | 本质还是条件分支，图结构表达更灵活 |
| Plan-Exec | for + function call | 顺序执行：先规划步骤列表，再逐个执行 |

## 编排的本质

业务编排 = **业务对不确定性的容忍程度 × LLM 能力差异**的被动适应——技术不复杂，作者观察到有团队用 Java 条件语句 + 线程池实现效果也不错。

## 关联

- 与 [[react-agent]]、[[plan-and-execute模式]]、[[workflow优先于agent]] 直接对应，为未来"ReAct vs Workflow"比较页提供分类素材。
- LangGraph 的定义已并入 [[langchain]] 页记录。
- 与 [[harness与workflow主导权之辨]] 构成"编排主导权"讨论的两端：本文还原代码本质，后者讨论主导权归属。