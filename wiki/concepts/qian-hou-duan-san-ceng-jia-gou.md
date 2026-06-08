---
type: concept
title: 前后端三层协作架构
tags: [harness-engineering, 前端, 后端, 协作架构]
related: [harness-engineering, san-chong-cai-ce-wen-ti, spec-driven-development]
created: 2026-06-08
updated: 2026-06-08
sources: ["别让AI瞎猜了用HarnessEngineering终结无限返工.html"]
---
# 前后端三层协作架构

[[harness-engineering|Harness Engineering]]将前后端的协作链路统一抽象为三层模型，强调**三层必须先后站稳**才能让agent角色清晰。

## 前端三层

| 层次 | 典型载体 | 核心作用 |
|------|---------|---------|
| **执行依据层** | Pencil/设计结构文件/设计说明 | 固定页面目录/组件层级/变量映射/状态/交互边界 |
| **状态暴露层** | Storybook/story文件 | 显式展示Default/Empty/Loading/Error/权限态 |
| **交付实现层** | 真实页面/路由/接口接入 | 业务路径接入 |

## 后端三层

| 层次 | 典型载体 | 核心作用 |
|------|---------|---------|
| **执行依据层** | docs/设计说明/接口说明/README/plan | 运行模式/IO/异常口径/非目标/回滚方式 |
| **状态暴露层** | 验证脚本/mock环境/试运行结果/可复现命令 | 成功/失败判定条件 |
| **交付实现层** | 实现/测试/联调/收口 | 代码是否进入正式业务路径 |

## 核心原则

- **执行语义冻结**：运行模式/IO/异常口径/非目标/回滚方式提前写清
- **状态冻结**：设计不仅冻结外观还要冻结状态，状态要进入可运行环境
- **工具可替代**：Pencil↔Figma，Storybook↔项目内Demo页，强调功能层而非具体工具

## Agent角色定位

当前两层稳定后，agent的位置才变得清楚——不是凭一句描述直接"生成页面"，而是在既定结构和状态之上**补实现、补细节、补接线**。

## 与Specflow的互补

[[specflow|Specflow]]的plan.md规格契约对应执行依据层（第一层），Storybook层则是Specflow未覆盖的补充维度。两者在三层架构中形成**层级包含关系**。