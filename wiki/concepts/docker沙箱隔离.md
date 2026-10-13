---
type: concept
title: Docker 沙箱隔离
tags: [沙箱, docker, 安全, afk]
related: [ralph-loop, hitl到afk渐进路径, agent-control-plane, openclaw三层沙箱纵深防御, coding-agent四特征, agents-md, docker沙箱隔离]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601270830]从ReAct到RalphLoopAIAgent的持续迭代范式.html"]
---

# Docker 沙箱隔离

Docker 沙箱隔离是 AFK（无人值守）Ralph 循环的安全边界机制（技巧 9）：通过 `docker sandbox run claude` 在容器内运行 Claude Code，使 Agent 获得编辑/执行/提交权限的同时，被限制在物理隔离的文件系统范围内。

## 安全问题

AFK Ralph 需要编辑/执行/提交权限，而人在键盘前才能拦截危险命令——无人值守时 `rm -rf ~` 这类命令无法被人工拦截，因此沙箱从可选变为必需。

## 隔离边界

- **能做**：编辑项目文件、运行命令、提交代码
- **不能做**：触及主目录、SSH 密钥、系统文件（仅挂载当前目录到容器）

## 权衡

- 代价：全局 AGENTS.md 与用户技能在沙箱内不加载（文章判断"对大多数 Ralph 循环没问题"）
- 分级定位：HITL 模式沙箱**可选**（人在观察）；AFK 模式尤其过夜循环沙箱**必备**——"防止失控代理的基本保险"

## 跨来源关联

- [[openclaw三层沙箱纵深防御]]：OpenClaw 的多层沙箱方案，与本页的单容器方案构成防御深度谱系
- [[agent-control-plane]]：沙箱是控制平面的执行边界组件
- [[coding-agent四特征]] 的"相对封闭"特征：沙箱是在编码场景内进一步收窄边界的手段