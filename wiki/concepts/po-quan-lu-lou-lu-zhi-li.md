---
type: concept
title: PO全链路泄露治理
tags: [架构, 泄露治理, 重构, 接口契约]
related: [si-ceng-jia-gou, ling-pai-qi-zhong-gou, ai-you-hao-yan-fa-gui-fan]
created: 2026-06-08
updated: 2026-06-08
sources: ["用Agent评测思路管理AICoding31万行代码AI重构的实践.html"]
---
# PO全链路泄露治理

[[agent-ping-ce-xi-tong|Agent评测系统]]重构中发现的典型架构问题及其三步治理方法。

## 问题

PO（Persistent Object）越过层级边界，从数据层直接泄露到业务层甚至展示层，导致层级耦合。

## 三步治理法

1. **补齐转换层**——在各层之间补充DTO/VO转换逻辑
2. **Application层重建接口契约阻断泄露**——利用[[si-ceng-jia-gou|四层架构]]的Application层作为阻断点
3. **修复上游参数依赖**——调整上游调用方，适配新的接口契约