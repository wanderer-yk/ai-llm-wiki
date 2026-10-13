---
type: concept
title: Skill 打包分发
created: 2026-10-13
updated: 2026-10-13
tags: [skill, 分发, 打包, 生态]
related: [skill开放标准, skill目录结构, skill生态知识体系, 生产级skill]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Skill 打包分发

Skill 打包分发是 Skill 从开发到交付他人使用的最后一环：[[skill-creator]] 提供 `python -m scripts.package_skill` 命令，将开发完成的 Skill 目录打包为 **`.skill` 单文件产物**，可分享给他人安装使用。

```bash
python -m scripts.package_skill path/to/code-review
# 生成 code-review.skill 文件，可分享安装
```

## 生态位

打包分发与 [[skill开放标准]] 共同构成 Skill 可复用性的基础设施：标准保证格式可移植，打包保证交付便利。来源参考资料列出 6 个开源 Skills 市场（skills.sh、skillsmp.com、openclaw/clawhub、qoder-community、alicloud-skills、hermes-agent）与 1060+ Skills 的 Awesome 列表，佐证分发生态的成型。clawhub 的列入为 [[openclaw]] 补充生态位证据。

## 待核问题

`.skill` 文件的内部打包格式（单文件是压缩包还是含清单结构）来源未给出。分发治理（版本管理、质量审核）与 [[生产级skill]]（腾讯视角：具备版本管理的可复用 SOP 封装）的对照待展开。
