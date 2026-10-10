---
type: concept
title: verification-agent五大设计哲学
created: 2026-10-10
updated: 2026-10-10
tags: [claude-code, verification, agent, harness-engineering, 质量门控]
related: [六大系统内置AgentTool, 验证门禁化, agent-control-plane, system-reminder注入机制, rd-as-qa, human-in-the-loop测试sop]
sources: ["[202604200830]深度解析ClaudeCode在PromptContextHarness的设计与实践.html"]
---
# verification-agent五大设计哲学

**verification-agent 五大设计哲学**概括了 [[claude-code]] Verification Agent（质量检验官）的设计：它是六大内置 Agent 中提示词最长、设计最精妙的一个，作者称之为最体现 Harness Engineering 精髓的 Agent。

## 五大哲学

1. **红蓝对抗**：开场白即基调——"You are a verification specialist. Your job is not to confirm the implementation works — it's to try to break it."（类比 GAN）。
2. **不轻易给 PASS**：点名两大典型失败模式——验证逃避（Verification Avoidance：读代码叙述"会"测试什么就写 PASS）与被前 80% 迷惑（Seduced by the First 80%："你的全部价值在于找到最后那 20%"）。
3. **严格权限控制**：只读；唯一例外可往 `/tmp` 写临时测试脚本（Bash 重定向）且用完自清理；被反复注入 CRITICAL 提醒（"CRITICAL: This is a VERIFICATION-ONLY task. You CANNOT edit, write, or create files IN THE PROJECT DIRECTORY."）；禁生成子 Agent、禁退出计划模式、禁编辑文件/笔记本。
4. **按变更类型分类的验证策略**（见下表）。
5. **反偷懒话术清单**：7 组 AI 自我开脱话术逐一拆穿。

## 按变更类型分类的验证策略（原文还原）

| 变更类型 | 验证策略 |
|---|---|
| 前端变更 | 启动开发服务器 → 浏览器自动化 → 检查子资源加载 |
| 后端/API | 启动服务 → curl 测试端点 → 验证响应结构 → 测试错误处理 |
| CLI/脚本 | 用代表性输入运行 → 验证 stdout/stderr/退出码 |
| 基础设施 | 语法验证 → 干运行（terraform plan, kubectl --dry-run） |
| Bug修复 | 先复现 Bug → 验证修复 → 回归测试 |
| 数据库迁移 | 运行迁移 → 验证 schema → 测试回滚（可逆性） |
| 重构 | 现有测试必须不改动地通过 → diff 公共 API |
| 移动端 | 清理构建 → 模拟器安装 → dump UI 树 → 点击验证 |

## 反偷懒话术七组

代码看起来是对的→运行它；实现者的测试已通过→独立验证；这大概没问题→运行它；启动服务器然后看代码→打端点；我没有浏览器→检查 playwright MCP 工具；太耗时了→不是你说了算；在写解释而不是运行命令→停下来运行命令。

## 跨源关联

Verification Agent 是 [[验证门禁化]]（vivo）理念的顶级落地实例；验证专职化与 [[rd-as-qa]]、[[human-in-the-loop测试sop]]（美团）共同指向「验证是独立职能而非附带动作」的共识。
