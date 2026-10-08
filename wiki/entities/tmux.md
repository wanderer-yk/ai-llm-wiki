---
type: entity
title: tmux
tags: [终端, 开源工具, agent-teams]
related: [claude-code, SubAgent与AgentTeams双模式]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202605090830]Harness实践让Agent自动制作知识讲解视频.html"]
---
# tmux

（公开背景）tmux 是开源的终端复用器（terminal multiplexer），可在单一物理终端中创建多个持久会话、窗口与分屏面板。在本文中，tmux 承担双重角色：

1. **启动持久会话**：先运行 `tmux` 进入会话，再执行 `claude --dangerously-skip-permissions` 启动 Claude Code。
2. **Agent Teams 组员面板可视化**：配合 Claude Code Agent Teams 后，每个组员出现在不同终端面板，可同时观察 Reviewer 审查与 Developer 修改。

## 安装与配置

```
brew install tmux
```

需在配置中为 Claude Code Agent Teams 添加 tmux 适配项；**具体键值仅在原文截图中，正文未给出文本**（存疑保留项）。另：原文正文曾将 tmux 误拼为 "tumx"，以代码块为准。