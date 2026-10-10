---
type: entity
title: RDSHermes
tags: [hermes-agent, 阿里云, RDS, 数据库, agent产品]
related: [hermes-agent, 组织级自进化, 密钥托管凭证隔离, 团队治理写操作二次确认, skill安全扫描统一门禁, skill轻量索引按需加载, 用得越久越好用, openclaw]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604230830]深入源码HermesAgent如何实现SelfImproving.html"]
---
# RDSHermes

RDSHermes 是据来源文章（[[sources/[202604230830]深入源码HermesAgent如何实现SelfImproving|《深入源码：Hermes Agent 如何实现 "Self-Improving"》]]，厂商侧自述语境）所述，将 [[hermes-agent]] 的自进化能力产品化、面向"不写代码的人"（尤其数据库运维团队）的服务化版本。文章给出一句话定位："开源 Hermes 是给开发者的引擎，RDSHermes 是给整个团队的成品车"，支撑论据是"不是所有人都会写 `config.yaml`，但所有人都会打字"。产品已上线阿里云 RDS AI 应用市场（免费试用）。**注意：本文关于 RDSHermes 的全部能力与效果描述均为厂商自述，未经第三方验证，引用需标注归因。**

## 核心能力：四件套

1. **数据库安全纳管**：支持 MySQL/PostgreSQL/SQL Server/MariaDB 一键接入 RDS 实例；密码提交瞬间加密；可设只读模式（"Agent 能查但不能改"）。
2. **身份认证托管**（见 [[密钥托管凭证隔离]]）：云凭证 AK/SK 由网关代理鉴权、加密托管，密钥不落盘、不暴露给 Agent 也不暴露给用户；对照开源版将 AK/SK 写进环境变量或配置文件。
3. **内置数据库专业技能**：Skill Hub 预装智能巡检、慢 SQL 诊断、索引优化等领域 Skill，解决自进化的冷启动问题；叠加 Agent 自身自进化实现"两条腿走路"（预装解决冷启动 + 自进化解决越用越强）。
4. **全链路监控审计**：Token 消耗可监控、安全事件有告警；写操作需二次确认才执行，会话可追溯、可审计（见 [[团队治理写操作二次确认]]）。

## 与开源 Hermes Agent 的门槛对比（文章原文表格）

| | 开源 Hermes Agent | RDSHermes |
|------|------------------|-----------|
| 开始使用 | 命令行安装，手写 config.yaml | 控制台一键开通，零配置 |
| 对话界面 | 终端 CLI | 内置 WebUI，打开浏览器就能对话 |
| 接入 IM | 内置 Gateway，config.yaml 配凭证后命令行启动 | 控制台里填个 App ID 就完成 |
| 数据库连接 | 手动配连接串，密码明文写配置 | 一键接入 RDS 实例，密码自动加密 |
| 云凭证管理 | AK/SK 写进环境变量或配置文件 | 加密托管，网关代理鉴权，密钥不落盘 |
| 技能管理 | Agent 自动创建，磁盘文件 | Skill Hub 预装专业技能 |

## 组织级自进化

开源 [[hermes-agent]] 的经验积累在单用户 `~/.hermes/` 目录；RDSHermes 将 Skill 存储搬到云端，一个 DBA 踩过的坑全团队 Agent 都能绕过——自进化从单点升级为组织级（见 [[组织级自进化]]，厂商自述）。与 [[知识复利效应]]（有赞共享技术）同族。

## 迁移与生态

提供 `hermes claw migrate` 一条命令，从 [[openclaw]] 或 RDSClaw 导入全部配置和记忆数据；配合 [[hermes-agent]] v0.6.0 的 Profiles 多实例、MCP Server Mode，作者称"OpenClaw 用户的切换门槛已经被系统性地拆掉了"。

## 效果声明（无测量口径，需归因）

- DBA 在飞书群 @一下，晨间巡检从 40 分钟缩短到 2 分钟。
- 市场部同事通过 WebUI 一句话查询渠道数据。
- 开发者排查数据库问题不再等待 DBA 排期。