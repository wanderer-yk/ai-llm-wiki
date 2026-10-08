---
type: entity
title: MiniMax
tags: [大模型, tts, 国产模型, 供应商]
related: [MMX-CLI, CC-Switch, claude-code, web-video-presentation]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202605090830]Harness实践让Agent自动制作知识讲解视频.html"]
---
# MiniMax

（独立可查证的背景资料：MiniMax 是中国的大模型公司，提供语言模型、语音合成、图片与视频生成等多模态服务。）在本文工具链中，MiniMax 承担双重角色：一是经 CC Switch 接入 Claude Code 的替代 LLM，二是语音合成（TTS）引擎。

## LLM 角色

- 作者因国内正常使用 Claude Code 困难（被封多个账号）而选用 MiniMax 作为替代模型。
- Token Plan 套餐与 Claude Code 适配好，作者订阅 Plus 极速版（速度快、量大、性价比高）。
- CC Switch 中模型名配置为 `MiniMax-M2.7`。
- 实战结论：多 Agent 并行调度"一定要选 Agent 能力非常强的模型"，否则易错。

## TTS 角色

- Token Plan 附带多模态套餐（图片/语音/视频），每日 9000 字语音合成额度"做两篇文章完全够用"。
- 实战音色：`Chinese (Mandarin)_Gentleman 温润男声`。
- 存在速率限制，通过串行逐条合成规避；已生成文件自动跳过实现断点续跑（详见 [[MMX-CLI]]）。