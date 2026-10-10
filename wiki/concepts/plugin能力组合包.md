---
type: concept
title: plugin能力组合包
tags: [plugin, extensibility, platform, claude-code]
related: [动态能力面稳定内部对象, mcp翻译收敛, skill能力声明对象]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604150830]ClaudeCode源码拆解从启动到多Agent扩展层.html"]
---
# plugin能力组合包

plugin能力组合包是 Claude Code 扩展层的三类能力来源之一：plugin 是能力的组合包，可携带能力单元、hooks、协议接入、语言服务、代理定义、输出风格、设置项等。

其加载层被定性为「小型平台分发问题」，涉及六个环节：来源、校验、缓存、版本、策略、启停。这意味着插件机制一旦正式化，就不再是简单的文件加载，而需要一套完整的分发治理。

plugin 与 MCP（[[mcp翻译收敛]]，协议接入）、Skills（[[skill能力声明对象]]，能力声明）共同构成扩展层，服从 [[动态能力面稳定内部对象]] 的收敛原则：无论插件生态多热闹，进入内部的都是少数稳定对象。
