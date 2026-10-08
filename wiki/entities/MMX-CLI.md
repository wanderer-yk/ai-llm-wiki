---
type: entity
title: "MMX CLI（MiniMax CLI）"
tags: [cli, tts, minimax, 工具封装]
related: [MiniMax, claude-code, web-video-presentation, 自动化决策层级, mcp工具封装模式]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202605090830]Harness实践让Agent自动制作知识讲解视频.html"]
---
# MMX CLI（MiniMax CLI）

MMX CLI 是 MiniMax 官方提供的多模态命令行工具（仓库：`https://github.com/MiniMax-AI/cli`），将图片、语音、视频等多模态能力封装为 Agent 可直接调用的 `mmx` 命令。它是本文工具链的第四个工具，承担口播稿的批量语音合成，使 Agent"不用写一行代码"即可调用语音合成能力。

## 安装与验证

安装方式为向 Claude Code 发送一句话提示（逐字）：

```
帮我安装 MiniMax CLI：https://github.com/MiniMax-AI/cli
我的密钥是 sk-cp-xxxxx
```

本地执行 `mmx`，能列出信息即安装成功。

## 批量口播合成：两步流程与恢复机制

| 环节 | 内容 |
|------|------|
| 第一步 | 从所有章节抽取口播文本清单，人眼校对错字/漏句/断句；口播 Step 与网页 Step 一一对应 |
| 第二步 | 逐条合成音频，脚本串行执行以规避 MiniMax 速率限制 |
| 断点续跑 | 已生成文件自动跳过，中途断了不用重来 |
| 产物结构 | 每章一个子目录，每步对应一个音频文件，文件名与步骤编号一一对应 |

串行执行与断点续跑是 [[Harness六大核心部分]] 中"约束与恢复"在音频环节的实例；其"CLI 封装为 Agent 可用工具"的思路与 [[自动化决策层级]]、[[mcp工具封装模式]] 同族。音频清单文件、步骤编号文件名延续了 [[文件化工作记忆]] 的"状态写进文件"思路。