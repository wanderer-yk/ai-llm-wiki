---
type: entity
title: program.md 规则核心
tags: [program-md, 规则文件, 权限边界, agent章程]
related: [karpathy, autoresearch, smallnest-autoresearch, agent权限边界清单, agent-control-plane, blocker-gate, 人的参与程度反映领域特征]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# program.md 规则核心

program.md 是 autoresearch 体系的规则核心：一份由人类维护的"章程/宪法"文件，定义实现规则、权限边界、代码规范与质量标准，是 Agent 自主运行的全部约束来源。Karpathy 原版中它相当于给 Agent 的"研究章程"；在 [[smallnest-autoresearch]] 中它是目录树第一项，注释即为"宪法"。

## 内容构成（smallnest/autoresearch 版）

1. **权限边界清单**：可改/不可改的显式清单（见 [[agent权限边界清单]]），核心是执行权与发布权分离
2. **Go 代码规范 5 条**：遵循 Effective Go + Go Code Review Comments；gofmt + goimports + golangci-lint；包名小写、文件名下划线、导出大写；接口用 er 后缀（Reader, Handler）；错误用 fmt.Errorf 包装提供上下文
3. **测试规范 5 条**：所有新功能必须有单元测试；覆盖率 ≥ 70%；表格驱动测试；命名 Test\<Function\>_\<Scenario\>；禁止 time.Sleep、外部依赖、全局状态、硬编码端口

## 人工介入点

最佳实践第 2 条明确："效果不够理想……就可以修改这个文件"——人的介入方式是**循环外的规则调优**，而非循环内的逐轮检查。这与 [[人的参与程度反映领域特征]] 一致：软件开发介于 ML 全自主与 Skill 每轮暂停确认之间，介入点收敛为这一份文件。

## 失效后果与方法论定位

六条核心原则的推演指出：无 program.md 则 Agent 越权（见 [[六条核心原则]]）。在 wiki 概念谱系中，program.md + 权限边界 + 循环外人工调优共同构成 [[agent-control-plane]]（控制平面）的直接实例，其"未达标不得进入下一环节"的门控属性与 [[blocker-gate]] 同族。
