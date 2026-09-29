---
type: entity
title: Harness Template
tags: [harness-engineering, github, 项目模板, 开源工具]
related: [harness-engineering, 数据库团队]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202605141200]别让AI瞎猜了用HarnessEngineering终结无限返工.html"]
---
# Harness Template

[[harness-engineering]] 的配套项目模板包，托管于 GitHub 仓库 **SisyphusSQ/harness-template**，由爱奇艺 [[数据库团队]] 提供。

## 定位

模板包不替换原项目工程结构，而是在旁边补一层协作和验证入口（"协作补层策略"）。提供的是可复用项目结构，而非必须原样照搬的文件，初始化时应适配目标项目的技术栈、任务系统和团队约定。

## 核心文件结构

| 文件/目录 | 职责 | 常见误用 |
|-----------|------|----------|
| `AGENTS.md` | 入口地图，导航+边界说明 | 写成项目百科淹没真正入口 |
| `docs/harness/control-plane.md` | 控制面，任务收集/冻结/切分/实现/验证/评审/回写七步 | 只写流程名不写判断条件 |
| `docs/harness/project-constraints.md` | 项目级规则登记+检查状态 | 将未机械化规则说成已强制执行 |
| `.agent/PLANS.md` | 复杂任务计划协议+范围冻结 | 只写流程口号不写真实代码入口 |
| `docs/test/` | 验证步骤复用+副作用记录 | 只贴终端输出不说明前置条件 |
| `scripts/harness/` | 结构检查/计划检查/review gate 脚本 | 把业务测试塞进harness |

## 初始化流程

五步法：①交模板+项目给agent → ②agent只读解读不修改 → ③输出方案（新增/修改/保留/风险） → ④根据技术栈选择层级 → ⑤确认后执行并运行验证。核心原则："先理解后执行"。