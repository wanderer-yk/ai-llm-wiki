---
type: concept
title: 四层架构
tags: [架构, 重构, 分层, 工程规范]
related: [ling-pai-qi-zhong-gou, bian-pai-lei-yu-neng-li-lei, ai-you-hao-yan-fa-gui-fan]
created: 2026-06-08
updated: 2026-06-08
sources: ["用Agent评测思路管理AICoding31万行代码AI重构的实践.html"]
---
# 四层架构

[[agent-ping-ce-xi-tong|Agent评测系统]]重构后的标准分层架构，取代旧"按需求建包"的面条式结构。

## 四层定义

| 层次 | 职责 |
|------|------|
| **Starter** | 启动与配置 |
| **Application** | 应用服务层，接口契约 |
| **Infrastructure** | 基础设施层，外部依赖 |
| **Common** | 公共工具与共享模型 |

## 关键设计决策

### 编排类与能力类分离

[[bian-pai-lei-yu-neng-li-lei|编排类与能力类]]是职责边界的划分维度，共识沉淀为渐进式加载的[[skill|Skill]]。

### Application层接口契约

[[po-quan-lu-lou-lu-zhi-li|PO全链路泄露治理]]中，Application层重建接口契约是阻断泄露的关键步骤。

## 重构前后对比

- **重构前**：面条式包结构，按需求建包，缺乏层次分离
- **重构后**：标准四层域驱动结构，十余个核心包完成迁移