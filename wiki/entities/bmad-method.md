---
type: entity
title: BMAD-METHOD
tags: [spec-driven, ai-coding, 多代理, 开源方案]
related: [spec-driven-development, specflow, openspec, github-spec-kit, duo-agent-jiao-se-xie-zuo]
created: 2026-06-08
updated: 2026-06-08
sources: ["治愈CursorAI编程的幻觉用它就够了.html"]
---
# BMAD-METHOD

**BMAD-METHOD** 是一种全能型多代理协作工程框架，模拟完整的专家团（PM/架构师/QA），核心为**角色思维隔离**——各阶段专注单一专家视角，避免角色间逻辑干扰。

## 核心贡献

- **角色思维隔离**：PM 负责需求澄清，架构师负责技术建模，QA 负责质量验证，各角色独立思考和输出
- **多代理协作**：模拟完整的产品开发生命周期角色分工
- 内生审计机制：不同角色间的交叉审查天然提供质量保障

## 对 Specflow 的启发

[[specflow|Specflow]] 吸收了 BMAD-METHOD 的角色思维隔离思想，体现在四阶段工作流中每阶段分配独立专家角色（PM/架构师/工程师/知识管理员）的设计中。

## 在 Cursor 中的局限

- 多角色切换在 Cursor 单一会话中实现复杂
- 专家团模板对前端中后台场景过于笨重