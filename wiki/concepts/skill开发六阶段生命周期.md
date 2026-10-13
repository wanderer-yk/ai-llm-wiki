---
type: concept
title: Skill 开发六阶段生命周期
created: 2026-10-13
updated: 2026-10-13
tags: [skill-creator, 生命周期, 流程]
related: [skill-creator, skill评测数据管道, skill-creator三agent评估链, 像做机器学习一样做prompt工程]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Skill 开发六阶段生命周期

Skill 开发六阶段生命周期是 [[skill-creator]] 定义的 Skill 工程化开发闭环流程，从需求捕获到打包发布形成完整迭代回路。

## 六阶段

| 阶段 | 关键动作 |
|------|---------|
| 一：需求捕获 | 理解意图、明确触发场景、确定输出格式、区分客观可验证 vs 主观创意型 |
| 二：编写 Skill | 编写 SKILL.md（YAML frontmatter + 指令主体）+ 准备辅助资源 |
| 三：测试执行 | 设计 2-3 个测试用例 → 并行启动 with_skill/without_skill 两组子 Agent（A/B 测试）→ 等待期起草量化断言 → 捕获 timing 数据 |
| 四：评估与评审 | Grader 评分 → 聚合基准数据 → Analyzer 分析模式 → 生成 Eval Viewer → 用户浏览器评审 → 收集 feedback.json |
| 五：迭代改进 | 分析反馈 → 泛化改进方向（避免过拟合）→ 重写 Skill → 新 iteration 目录 → 回到阶段三 |
| 六：优化与发布 | Description 优化（run_loop.py）→ 训练/测试集分割 → 自动迭代改进描述 → 校验 → 打包 .skill 文件 |

## 实战 Walkthrough 计数修正

2.6 节以 code-review skill 为例的实战 walkthrough 文中前文称"五步实战"，实际为 **Step 1-8**（Step 6 迭代改进、Step 7 description 优化、Step 8 打包发布补全了评估后的迭代与发布阶段）——属来源计数松散问题（登记 [[ai工程量化效果声明追踪]]）。

## 关联

该生命周期是 [[像做机器学习一样做prompt工程]] 哲学的流程化表达；其交互成本问题（9 个确认节点）在 [[skill-creator六大局限]] 中被自我披露。
