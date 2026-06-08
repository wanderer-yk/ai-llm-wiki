---
type: concept
title: AI友好研发规范
tags: [规范, ai-coding, 工程实践, 约束]
related: [ren-ren-dui-qi-ren-ji-dui-qi, always-ji-bie-ai-rule, jian-jin-shi-zhong-gou, ji-shu-zhai]
created: 2026-06-08
updated: 2026-06-08
sources: ["用Agent评测思路管理AICoding31万行代码AI重构的实践.html"]
---
# AI友好研发规范

在AI Coding时代，研发规范的定位发生根本性变化：从"人与人的协作建议"升级为"约束AI产出、阻止系统长新债的基础设施"。

## 核心转变

### 传统规范 vs AI时代规范

| 维度 | 传统规范 | AI友好规范 |
|------|----------|------------|
| 目标 | 人与人协作对齐 | 约束AI产出质量 |
| 形式 | 文档、Wiki | [[always-ji-bie-ai-rule|always级别AI Rule]] |
| 执行 | 人工Review时参照 | AI编码过程中强制执行 |
| 更新节奏 | 低频 | 随业务高频迭代 |

## 关键要素

1. **必须可被AI理解并执行**——自然语言描述需精确无歧义
2. **必须always加载**——不依赖人记忆，AI始终遵守
3. **必须阻止新债产生**——每条规范对应一个可检测的约束条件
4. **必须随业务迭代**——[[ren-ren-dui-qi-ren-ji-dui-qi|人人对齐]]的共识持续更新

## 实践中的规范类型

- 工程分层规范（[[si-ceng-jia-gou|四层架构]]）
- 职责边界规范（[[bian-pai-lei-yu-neng-li-lei|编排类与能力类]]）
- [[po-quan-lu-lou-lu-zhi-li|PO全链路泄露治理]]规范
- 编码风格规范
- 数据库访问规范