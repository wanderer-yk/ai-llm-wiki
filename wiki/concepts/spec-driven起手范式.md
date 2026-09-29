---
type: concept
title: Spec-driven 起手范式
tags: [spec-driven, 项目初始化, 架构设计, qoder]
related: [动态spec机制, interview机制, spec-driven起手范式, 规格驱动ai开发, qoder]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202601242318]QoderQuest10把执行交给AI把选择留给人类.html"]
---
# Spec-driven 起手范式

"Spec-driven 起手范式"是 [[kiritomoe]] 在 [[ai网关压测项目]] 实践中推荐的新项目启动策略：对于新项目，起手阶段优先使用 spec-driven 模式产出完整架构方案，再进入编码执行阶段。

## 工作流程

1. **描述需求** — 用自然语言描述项目目标和核心需求
2. **[[interview机制|interview]]** — Quest 主动提问澄清模糊点
3. **生成 spec** — 产出完整架构方案（[[动态spec机制|动态 spec]]）
4. **审核 spec** — 人类以架构师视角审核规格
5. **执行** — Quest 根据 spec 自主编码

## 适用场景

- **新项目（非存量）** — 强烈推荐 spec-driven 起手，追求交付效率和引入 AI 原生技术栈
- **存量项目** — 推荐使用 Editor 模式，追求少量改动和确定性

## 实践验证

在 [[ai网关压测项目]] 中，作者以 spec-driven 起手，产出了包含施压程序、mock-llm-server、report-server 的完整架构方案，后续迭代中通过描述问题而非阅读代码修复 Bug，投入 3 天 + 8000 credits 完成全项目。

## 跨领域关联

- 是 [[规格驱动ai开发]] 在新项目场景下的具体应用
- 与 [[specflow]] 的全链路流程闭环一致——都强调先 spec 再执行
- 与 [[研发范式前移]] 的理念一致——将关键决策前移至需求设计期
- 与 [[先易后难陷阱]] 形成对照——spec-driven 起手是"先难后易"的正确路径