---
type: source
title: "【干货】从 Prompt、Context 到 Harness，工程的三次进化与终局之战"
authors: [李伟山]
year: 2026
url: ""
venue: 鹅厂技术派（微信公众号）/ 腾讯云开发者
tags: [prompt-engineering, context-engineering, harness-engineering, ai-coding, agent-system, 腾讯]
related: [工程三次进化框架, harness-engineering-李伟山版, context-engineering-李伟山版, prompt-engineering-李伟山版, harness衰变定律, f-harness, human-steer-agents-execute]
created: 2026-07-20
updated: 2026-07-20
sources: ["[202605201731]干货从PromptContext到Harness工程的三次进化与终局之战原创.html"]
---
# 【干货】从 Prompt、Context 到 Harness，工程的三次进化与终局之战

## 基本信息

| 字段 | 值 |
|------|-----|
| 作者 | 李伟山 |
| 发布平台 | 微信公众号「鹅厂技术派」 |
| 内容来源 | 腾讯云开发者 |
| 发布时间 | 2026-05-20 17:31 |
| IP 属地 | 广东 |
| 备注 | 注明使用 AI 辅助写作 |

## 核心论点

文章核心 meta 论点为"软件工程没有消失，它在进化"，提出 [[工程三次进化框架]]：Prompt Engineering → Context Engineering → Harness Engineering。三者分别解决"说清楚/给够信息/系统可靠"三个递进问题，层层嵌套、缺一不可。

## 全文结构（09 章）

### 引言：一个令人不安的问题

以 OpenAI 3-7 人小团队 5 个月内 AI 生成近 100 万行生产级代码为切入点，提出核心问题：工程师的价值到底在哪里？

### 第 01 章：理解起点——为什么和 AI 说话是一门学问？

阐述 LLM 底层逻辑为"极其擅长续写的系统"，"最有可能出现"≠"真正想要"。以道歉信三层约束递进为例，论证输出质量由约束条件决定。

### 第 02 章：第一次进化——Prompt Engineering

定义 [[prompt-engineering-李伟山版]] 本质为"加约束"过程，列举五种核心技术（Zero-shot / Few-shot / CoT / Role Prompting / Prompt Chaining）。论述 Prompt Engineering 的边际衰减规律：随 GPT-3 → GPT-4 / Claude 3 模型能力提升，精心设计 Prompt 的边际效益递减。

### 第 03 章：第二次进化——Context Engineering

以"金鱼记忆助理"思想实验引入，指出 LLM 本质是上下文窗口受限的失忆系统。核心技术手段包括：RAG（存索引不存知识）、上下文压缩三策略（滚动摘要+重要性评分+层次记忆）、单一事实来源纪律。引用 Lost in the Middle 研究发现。OpenAI 将巨型规范文件 `agent.md` 压缩为百行索引目录的实践。以"代码生成 Agent 失控"场景收束——论证 Prompt 和 Context 存在共同盲区（系统层面缺乏约束/验证/反馈）。

### 第 04 章：第三次进化——Harness Engineering

定义 [[harness-engineering-李伟山版]] 为"为 AI 系统设计约束/验证/反馈马具的系统工程"。核心公式：**完整 AI Agent 系统 = 大模型 + Harness；Harness 是除大模型之外的一切**。

**OpenAI 百万行代码实验**：初期瓶颈在于 Harness 设计而非模型能力。三大策略：上下文治理（Context Governance）、[[验证闭环]]（Verification Loop）、技术债清理（Tech Debt Cleanup）。其中验证闭环是质量保障核心——将"声称完成"变为"验证完成"。

**Anthropic F-Harness**：[[f-harness]] 三角色分工（Planner / Generator / Evaluator），解决 [[AI自恋问题]]——AI 倾向于给自己产出打高分的系统性偏差。

### 第 05 章：三者的嵌套关系

[[嵌套关系论-李伟山版]]：三者不是替代而是层层包裹的嵌套关系。Prompt 回答"我该跟模型说什么？"，Context 回答"模型该知道什么？"，Harness 回答"整个 AI 系统该如何可靠运转？"

### 第 06 章：Harness 的衰变定律

[[harness衰变定律]]：模型能力越强，所需 Harness 越简单。Claude 3.0 → Claude 3.5 升级使许多 Harness 规则自然失效。两层深意：第一，Harness Engineering 是当下的现实答案；第二，它可能是过渡性技术。实践建议集中在业务逻辑边界和外部环境接口两类不可替代场景。

### 第 07 章：新范式下的工程师角色

[[human-steer-agents-execute]] 工程哲学。[[工程师三职责]]：定方向（Steering）、搭架子（Harnessing）、做判别（Decision Making）。完整对比表（5 项维度）展示衡量标准从"个人产出"到"[[系统杠杆]]"的切换。

### 第 08 章：实践路线图

[[四步实践路线图-李伟山]]：Prompt 基础 → Context Engineering → Agent 系统设计 → [[动态harness思维]]。其中动态 Harness 思维被定义为"最难培养、也是最有价值的能力"。

### 第 09 章：结语——三次进化，一个目标

[[三次进化统一目标论]]：三次进化服务于同一目标——将 LLM 能力转化为可靠生产力。终极结论：工程师从"写代码的人"进化为"设计让 AI 把代码写好的系统的人"。

## 核心理论贡献

1. **[[工程三次进化框架]]**：Prompt → Context → Harness 的系统化演进框架
2. **[[harness衰变定律]]**：模型能力与 Harness 复杂度呈反比的规律
3. **[[可靠性边界论]]**：任务复杂度超过单 Agent 可靠性边界时多 Agent Harness 为唯一工程解法
4. **[[human-steer-agents-execute]]**：人类掌舵、Agent 执行的工程哲学
5. **[[系统杠杆]]**：工程师价值衡量标准的核心概念

## 与现有 Wiki 概念的关联与张力

- 本文 Harness Engineering（李伟山版）与爱奇艺 [[harness-engineering]]（[[数据库团队]]版）需系统对比——两者从不同视角（理论框架 vs 五要素工程化）探讨同一主题。
- 本文 Context Engineering 与 [[上下文工程]]（富城版"知识注入+数据注入+Agent接入"）和 [[三大武器库]]（binxiong 版）可互补。
- 本文"单一事实来源"与 [[多源分治策略]]（爱奇艺版"信息按职责分配到多个稳定位置"）可能构成互补而非矛盾。
- F-Harness Evaluator 与 [[高阶模型审查低阶模型]]（美团）形成跨来源验证体系。
- "Human Steer, Agents Execute"与 [[增强自我而非取代自我]]（zhiyuanfu）、[[自动化决策层级]]（zhiyuanfu）高度一致。
- "工程师价值向上迁移论"与 [[经验价值迁移]]（美团）形成跨来源共识。
- "系统杠杆"与 [[人工并发天花板]]（zhiyuanfu）形成互补。

## 开放问题

- 李伟山在腾讯的具体部门归属
- 鹅厂技术派与 [[腾讯技术工程]] 的关系
- Lost in the Middle 研究论文出处
- F-Harness 是否为 Anthropic 官方术语
- F-Harness $200 成本在常规开发中的可行性
- "可靠性边界"如何量化
- Harness 衰变速率是否有量化经验公式
- Claude 3.0 → 3.5 具体哪些 Harness 规则被废弃