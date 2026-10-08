---
type: entity
title: CC Switch
tags: [桌面工具, 模型切换, 配置, 开源]
related: [claude-code, MiniMax, ConardLi]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202605090830]Harness实践让Agent自动制作知识讲解视频.html"]
---
# CC Switch

CC Switch 是开发者 farion1231 开源的桌面端模型切换配置工具（GitHub Release v3.14.1），可为 Claude Code、Codex、Gemini CLI 等命令行 Agent 配置任意自定义模型供应商。在本文工具链中，CC Switch 用于将 Claude Code 的模型切换为 MiniMax，构成国产化替代接入链路的一环。

## 配置流程（本文逐字步骤）

下载地址：`https://github.com/farion1231/cc-switch/releases/tag/v3.14.1`

1. 点击"+"，选择预设 MiniMax 供应商；
2. 填入 API Key；
3. 将模型名称全部改为 `MiniMax-M2.7`；
4. 保存后在首页点击"启用"；
5. 终端输入 `claude` 即可直接使用。