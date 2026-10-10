---
type: concept
title: OpenClaw 三层沙箱纵深防御
tags: [openclaw, 安全, 沙箱, harness]
related: [openclaw, 多层安全护栏, agent-control-plane, 全生命周期hook机制, human-in-the-loop, clawhub技能安全管控]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践.html"]
---

# OpenClaw 三层沙箱纵深防御

OpenClaw 三层沙箱纵深防御是 [[openclaw]] Harness 安全护栏的核心机制：在文件系统、命令执行、网络访问三个维度分别设立彼此独立、互补的沙箱，底层再以操作系统最小权限兜底，构成"电子围栏"式纵深防御。其设计哲学是**不依赖模型自我约束，而以系统级强制力保障安全**。

结构（文字化）：

```
文件系统沙箱 → 命令执行沙箱（Security 白名单 / Ask 人工确认 / safeBins 豁免）
→ 网络访问沙箱（域名白名单 + 防泄露）→ OS 最小权限兜底（进程插件 + 可选编排服务）
→ 四防（注入/越权/泄露/篡改）
```

各层细节：

1. **文件系统沙箱**：将 Agent 禁锢于 Workspace 指定目录，越界读写直接阻断。
2. **命令执行沙箱**：Security 模式以白名单限制可执行命令；Ask 模式在关键节点暂停并请求人工确认（与 [[human-in-the-loop]] 的最终控制权衔接）；safeBins 豁免名单为只读工具放行，平衡效率与安全。
3. **网络访问沙箱**：白名单域名机制使 Agent 仅可达可信端点；防泄露机制确保即使命令执行成功，敏感数据也无法流出。
4. **OS 最小权限兜底**：运行时安全管控解耦为独立进程插件与可选编排服务，以操作系统级强制力兜底。

**四防**：防注入攻击（拦截恶意 Prompt 注入）/ 防越权调用（校验工具调用权限边界）/ 防敏感泄露（防 API Key、密码意外输出）/ 防恶意篡改（监控本地文件写操作）。

版本时效注意：作者评价 OpenClaw 早期 Harness 约束单薄、依赖模型"自觉"，近期才显著加强（如 ClawHub Skills 鉴权，见 [[clawhub技能安全管控]] 相关讨论），引用时须注意 2026-03 底的版本时点。与 HermesAgent 的 [[多层安全护栏]] 构成跨系统对照实现，是 [[agent-control-plane]]（权限/边界/审计）的最强具象证据之一。

来源：[[sources/[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践|[202604130830] 深度解析 OpenClaw]]。
