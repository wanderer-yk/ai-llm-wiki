---
type: concept
title: Harness 与 Workflow 主导权之辨
tags: [harness-engineering, workflow, agent架构, 控制权]
related: [workflow与agent控制权分界, harness四强制约束, agent裸奔四问题, harness-engineering, prompt-context-harness三阶段]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践.html"]
---

# Harness 与 Workflow 主导权之辨

Harness 与 Workflow 主导权之辨回答"既然都要限制 Agent 自由发挥，Harness 和 Workflow 有何本质区别"：Workflow 是硬编码固定路径（Step A→B→C），模型只是节点上的执行者，确定性高但异常易断裂，**主导权在人**；Harness 是框架层面的动态软约束，保留 Agent 的自主规划（Planning）与循环迭代（Looping），**主导权在 AI**。

两者的目标一致（限制自由发挥、提升可控性），本质区别仅在主导权归属。作者的判断：基础大模型越强，Workflow 的弊端就越大于优势——更强的模型被硬编码路径束缚反而是浪费；Harness 更符合当前时间窗口。这为 [[prompt-context-harness三阶段]] 中"Design How Controlled"的必要性提供了路径选择论据。

本概念与侑夕的 [[workflow与agent控制权分界]]（控制权光谱视角）形成跨来源呼应，与 [[harness-engineering]]（爱奇艺五要素方法论）是通用概念与具体方法论的关系；实施层面的硬约束清单见 [[harness四强制约束]]，无 Harness 的后果见 [[agent裸奔四问题]]。

来源：[[sources/[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践|[202604130830] 深度解析 OpenClaw]]。
