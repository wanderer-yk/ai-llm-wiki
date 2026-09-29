---
type: concept
title: Bridge Rule
tags: [ai上下文, 桥接规则, skill]
related: [codebuddy, ai-native研发模式, openspec]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604031907]当整个团队开始0人工Coding一份万字AINative研发实战手册.html"]
---
# Bridge Rule

解决 AI 工具链之间"链路断裂"问题的桥接规则设计模式。当 AI 不主动感知项目配置文件（如 config.yaml）时，通过生成轻量"指路牌"文件引导 AI 去读取正确的规则源。

## 问题背景

AI 编码工具默认不知道要读取项目的 `openspec/config.yaml` 配置文件，导致 AI 生成的代码缺乏项目级约束（技术栈、代码规范、业务背景），形成"链路断裂"。

## 设计方案

在 `.codebuddy/rules/` 目录生成轻量级指路牌文件：
- **只做"指路"**，不复制规则内容，保持 config.yaml 为 SSOT
- 使用 `alwaysApply: true` 确保自动加载

## 版本迭代验证

| 版本 | alwaysApply 设置 | 效果 |
|------|-----------------|------|
| v0.4.2 | false | 实测偶尔漏判 |
| v0.4.3 | true | 问题彻底解决 |

## 核心设计原则

- **SSOT 保持**：指路牌文件只含引用路径，config.yaml 仍是唯一真实来源
- **无感知加载**：alwaysApply: true 确保 AI 每次启动都加载规则
- **由 Skill 自动生成**：openspec-installer 在 Step 6a-1 自动创建，无需手动配置

## 更广泛的意义

Bridge Rule 是一个通用的"AI 上下文桥接"设计模式，可推广到任何需要 AI 感知项目配置但默认不感知的场景。它直接呼应了 [[ai-native研发模式]] 四大痛点中的"上下文缺失"——AI 无法主动感知项目配置。