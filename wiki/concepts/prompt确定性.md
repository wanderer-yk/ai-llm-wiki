---
type: concept
title: Prompt 确定性
tags: [prompt工程, 任务下发, harness-engineering]
related: [子任务CLI化, 主agent转述失真, 批判性evaluator校验]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务.html"]
---
# Prompt 确定性

百度Geek说《Harness Engineering》[[子任务CLI化]] 的第一大好处：子任务 Prompt 由 `build-prompt.js` **程序化组装**，而非由主 Agent 转述。

## 程序化组装的五个组成部分

1. 任务详情；
2. 规则约束；
3. 输入文件列表；
4. 输出格式；
5. 验证标准。

## 解决的问题

主 Agent"理解"指令后再转发时会产生偏差：自行改写措辞、塞入自己推断的上下文、直接粘贴文件内容（→ [[主agent转述失真]]）。程序化组装保证每个 subAgent 收到的 Prompt 逐字节一致、可复现、可审计。

## 体系位置

Prompt 确定性是长程任务可靠性的入口保障：下发不确定，则后续的完成判定（[[任务状态自描述]]）与校验（[[批判性evaluator校验]]）都在放大噪声。