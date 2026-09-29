---
type: concept
title: 多agent逆向工程初始化
tags: [多Agent, 逆向工程, 知识初始化, AI-Coding]
related: [分布式wiki架构, 知识准入控制, 上下文防火墙, knowledge-wiki, 多智能体消息机制]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604071830]KnowledgeWiki面向AI的项目知识层建设实践.html"]
---
# 多agent逆向工程初始化

有赞 Knowledge Wiki 的知识库初始化方案：通过多 Agent 协作对现有代码库进行逆向工程，自动生成初始 wiki 知识库。

## 三角色协作

| 角色 | 职责 |
|------|------|
| **Controller** | 全局规划、任务分配、质量把控 |
| **Implementer** | 逐应用扫描建模、提取知识条目 |
| **Reviewer** | 审查输出质量、执行候选三去向判断 |

## 四阶段流程

1. **全局扫描**：Controller 扫描 workspace 整体结构，确定应用列表和优先级
2. **逐应用建模**：Implementer 深入每个应用，提取流程、概念、排障路径等知识
3. **Workspace 聚合**：将各应用知识汇总，执行归并、去重、提炼
4. **审查收尾**：Reviewer 执行 [[分布式wiki架构]] 中的候选三去向原则，确保知识精炼

## 上下文防火墙

每个 SubAgent 只接收最小信息包——这是 [[harness-engineering]] 思想在 Agent 编排层面的应用。核心原则：

- 不把整个代码库一次性塞给 Agent
- 每个阶段只传递该阶段需要的上下文
- 防止 Agent 在海量信息中迷失或产生幻觉

## 与上下文 ≠ Maven 模块的关系

初始化过程中，上下文划分基于业务流程（Flow Cluster Map）而非 Maven 模块。这意味着：
- 一个上下文可能跨多个 Maven 模块
- 一个 Maven 模块可能包含多个上下文
- 必须从业务视角而非技术视角组织知识

## 实践要点

- 异步链路（Producer/Consumer）优先初始化——因为 AI 无法从代码直接串起这类链路
- 初始化是"先穷举再归并"的过程，即知识蒸馏
- 最终产物是 [[分布式wiki架构]] 中的 workspace→app→context 三级目录结构