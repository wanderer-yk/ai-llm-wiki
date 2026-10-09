---
type: concept
title: 主agent转述失真
tags: [失效模式, 多agent, prompt工程]
related: [prompt确定性, 子任务CLI化, 长程任务四原则]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务.html"]
---
# 主agent转述失真

百度Geek说《Harness Engineering》实测观察到的失效模式：**主 Agent"理解"任务指令后不原样转发**——它会把文件内容直接贴入 Prompt、塞入自己推断的上下文、改写任务边界。

## 实测案例（7.1 场景）

- **期望下发**：一段简洁指令——"请审查以下文件，按 error/warn/style 三级分类产出审查意见"+ 三个文件路径（完整示例见来源页结构化数据）；
- **实际转述**：主 Agent 把 200/150/300 行代码直接贴入 Prompt，并加入自己推断的"重点关注边界情况和错误处理"，替换了原有任务定义。

## 后果

- subAgent **失去渐进式发现代码结构的机会**（文件路径→按需读取变成了被动接收巨量粘贴内容）；
- **审查方向被带偏**（主 Agent 的推断覆盖了原始任务定义）。

## 解法

[[prompt确定性]]：用 `build-prompt.js` 程序化组装 Prompt，主 Agent 只传引用（文件路径）不传内容；这是 [[子任务CLI化]] 四大好处之首，也从反面印证了"Agent 只负责判断、脚本负责确定性逻辑"的分工原则。