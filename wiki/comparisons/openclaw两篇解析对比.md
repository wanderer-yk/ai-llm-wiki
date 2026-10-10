---
type: comparison
title: openclaw两篇解析对比
tags: [openclaw, 跨来源对比, 对比]
related: [openclaw, prompt-context-harness三阶段, 自进化记忆管线, 原生记忆不确定性链路, openclaw双层记忆系统]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践.html", "[202604151800]OpenClaw长期记忆优秀管线与玄学效果.html"]
---
# openclaw 两篇解析对比

千问AI平台两篇 OpenClaw 分析（飞樰三维度篇 vs 城决长期记忆篇，app 口径「最强 comparison 候选」）——切面互补而非重叠。

## 两篇切面

| | 飞樰·三维度篇（04-13） | 城决·长期记忆篇（04-15） |
|---|---|---|
| **切面** | Prompt/Context/Harness 全景架构 | 记忆子系统纵深（原生管线 vs RDSClaw 插件） |
| **核心产出** | 系统提示词 23 模块、context 三段构成、三层沙箱、heartbeat 机制 | 睡眠隐喻整合族（light-sleep/REM/Dreaming/Deep-Sleep）、LoCoMo10 +13.90% |
| **证据等级** | 源码级（References 5 条全部兑现） | 插件实测+自报评测（加权口径已核实） |
| **对三剑批评的态度** | 证实原生有自动记忆压缩/衰减（反证「无压缩」论） | 证实记忆效果不确定性高（「玄学」佐证「有但不稳」） |

## 合并结论

两篇合并给出 OpenClaw 原生记忆的完整图景：**机制存在**（双层记忆+时间衰减+双触发压缩，三剑「无自动机制」论不成立）但**效果不确定**（原生链路玄学 vs RDSClaw 插件确定性管线——这正是插件生态的价值空间）。对 [[用得越久越好用]] 的对比链路修正详见 [[hermes-agent-vs-openclaw-vs-claude-code]] 归因警示证据升级节。
