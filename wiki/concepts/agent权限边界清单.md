---
type: concept
title: Agent 权限边界清单
tags: [autoresearch, 权限, control-plane, 安全]
related: [program-md规则核心, smallnest-autoresearch, agent-control-plane, blocker-gate, 四阶段优化循环]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# Agent 权限边界清单

Agent 权限边界清单指 [[smallnest-autoresearch]] 在 [[program-md规则核心]]（program.md）中以"可以 / 不可以"两条清单显式划定 Coding Agent 的操作范围，并借"推送远程由 run.sh 统一处理"实现执行权与发布权分离。它是 [[agent-control-plane]]（权限/边界/审计）在开源项目中的直接实例，与爱奇艺 [[blocker-gate]] 的"未满足条件即强制阻断"共同代表 Agent 行为控制面的两种实现路径——清单约束 vs 门控阻断。

## 清单原文

```text
Agent 可以:
  ✓ 修改 internal/, cmd/
  ✓ 创建/修改测试文件
  ✓ 运行测试和 lint
  ✓ 创建本地分支和 commit
  ✓ 在 workflows/ 记录日志
Agent 不可以:
  ✗ 修改 go.mod, .github/, Makefile, CI/CD
  ✗ 删除任何现有文件
  ✗ 推送到远程仓库（由 run.sh 统一处理）
  ✗ 关闭 Issue
  ✗ 修改 autoresearch/ 规则文件
```

## 设计要点

- **执行权与发布权分离**：Agent 有本地 commit 权但无推送权，push/PR/merge 收敛到 [[四阶段优化循环]] Phase 3 由 run.sh 统一执行——Agent 行为与发布行为解耦。
- **防自举越权**：禁止修改 `autoresearch/` 规则文件自身，防止 Agent 修改约束自己的规则。
- **保护仓库现状**：不可删除任何现有文件、不可关闭 Issue、不可触碰 CI/CD 与依赖清单。

program.md 中除权限边界外，还包含 Go 代码规范与测试规范要点（遵循 Effective Go + Go Code Review Comments、gofmt + goimports + golangci-lint、覆盖率 ≥ 70% 硬门槛等），完整内容见 [[program-md规则核心]]。
