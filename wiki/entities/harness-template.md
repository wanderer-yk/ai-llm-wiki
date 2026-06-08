---
type: entity
title: harness-template
tags: [开源项目, 模板, harness-engineering, GitHub]
related: [harness-engineering, shu-ju-ku-tuan-dui]
created: 2026-06-08
updated: 2026-06-08
sources: ["别让AI瞎猜了用HarnessEngineering终结无限返工.html"]
---
# harness-template

**GitHub地址**：SisyphusSQ/harness-template
**维护方**：[[shu-ju-ku-tuan-dui|爱奇艺数据库团队]]

[[harness-engineering|Harness Engineering]]的开源项目模板仓库，提供可复用的项目结构模板。模板定位为在原项目旁边补一层协作与验证入口，不替换原工程结构。

## 模板目录结构

| 路径 | 功能 |
|------|------|
| `AGENTS.md` | 入口地图，告诉agent项目结构、入口和验证命令 |
| `docs/harness/control-plane.md` | 任务全生命周期控制平面（收集→冻结→切分→实现→验证→评审→回写） |
| `docs/harness/project-constraints.md` | 项目级规则登记与检查状态 |
| `.agent/PLANS.md` | 复杂任务计划协议（Scope/Non-Goals/Validation/Rollback） |
| `docs/test/` | 验证步骤复用与副作用记录 |
| `scripts/harness/` | 结构检查/计划检查/review gate脚本固化 |

## 初始化原则

1. 先让agent解读模板，理解模板意图
2. 再让agent理解目标项目
3. 最后对齐两者，按技术栈选择初始化层级
4. 确认后执行并运行验证

## 常见误用

- AGENTS.md写成项目百科导致入口被淹没
- PLANS.md只写流程口号不写真实代码入口
- project-constraints.md把尚未机械化的规则说成已强制执行
- 把业务测试全部塞进harness检查