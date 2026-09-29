---
type: entity
title: AI 网关压测项目
tags: [压测, ai网关, higress, k8s, 实践案例]
related: [qoder, kiritomoe, higress, skill环境感知, spec-driven起手范式, 自然语言触发心流]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202601242318]QoderQuest10把执行交给AI把选择留给人类.html"]
---
# AI 网关压测项目

AI 网关压测项目是 [[kiritomoe]] 使用 [[qoder]] Quest 1.0 从 0 到 1 构建的压测方案系统，用于对 [[higress]] AI 网关进行性能评估。该项目是 Qoder Quest 全流程能力（spec-driven 起手 + Skill 环境感知 + 自然语言触发心流）的完整实证案例。

## 系统组成

- **施压程序** — 核心压测引擎，发起对 AI 网关的请求
- **mock-llm-server** — 模拟 LLM 后端，支持流式响应
- **report-server** — 数据采集与报告生成服务
- **触发脚本** — 任务调度与执行入口

## 压测指标体系

传统 API 网关的压测指标（QPS、RT）不适用于以长连接和 SSE 流式响应为特征的 AI 网关。作者提出适用于 AI 网关的压测指标体系：

| 维度 | 传统 API 网关 | AI 网关 |
|------|--------------|---------|
| 请求特征 | 短连接 | 长连接 + SSE 流式 |
| 响应模式 | 同步返回完整结果 | 流式逐字返回 |
| 核心指标 | QPS、RT | TTFT（首字延迟）、TPOT（单字生成时间）、并发连接数 |

## 开发过程

### Spec-driven 起手

以 [[spec-driven起手范式]] 起手，产出完整架构方案后再进入执行阶段。

### 自然语言触发修复

迭代中遇到真实问题（如未记录真实 token、O(n²) 拼接数据导致大并发超时、PVC 数据写入超限）时，通过 [[自然语言触发心流]] 向 AI 描述问题并提供运行时环境，AI 大概率一次性修复正确。

### Skill 环境感知部署

利用 [[skill环境感知]] 机制，将 Dockerfile、Helm Chart、云端 K8s 连接配置封装为 Skill，使 AI 直接操作底层 [[k8s集群]]，完成 rollout 更新等运维操作。

## 成果

- 投入 3 天 + 8000 credits
- 自动生成《Higress AI 网关性能评估报告》
- 实现从代码编写到 K8s 部署到报告生成的全流程自动化

## 跨领域关联

该项目是 Wiki 中少有的"从 0 到 1 全新项目"实践案例（多数案例如美团 31 万行重构为存量改造），与 [[规格驱动ai开发]] 的 SDD 流程和 seanguo 的 [[十一阶段后台开发流程]] 形成互补参照。