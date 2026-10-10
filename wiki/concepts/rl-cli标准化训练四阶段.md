---
type: concept
title: rl-cli 标准化训练四阶段
tags: [rl训练, 工程化, cli, hermes-agent, 验证门禁]
related: [hermes-agent, grpo算法, research-ready训练闭环, toolcontext真实验证, 验证门禁化, 双轨校验]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践.html"]
---
# rl-cli 标准化训练四阶段

rl-cli 标准化训练四阶段是 [[hermes-agent]] 通过 `rl_cli.py` 将 RL 训练工程化为标准化流程的方法：**Discover & Inspect → Create & Configure → Test & Train → Evaluate**，使模型训练像软件开发一样有固定的操作序列与防错防线。

## 四阶段详解

| 阶段 | 关键函数/要点 |
|---|---|
| Discover & Inspect | `rl_list_environments()` 列出环境；inspect 检查 `load_dataset()` / `score_answer()` / `get_next_item()` / `system_prompt` / `config_init()` |
| Create & Configure | `rl_select_environment` 选择环境；`rl_edit_config` 编辑配置 |
| Test & Train | **强制 `rl_test_inference`**（防错防线，防止训练配置错误浪费算力）→ `rl_start_training` 启动训练 → `rl_check_status` 检查状态（训练异步，建议间隔≥30 分钟） |
| Evaluate | `rl_get_results` 获取结果 + WandB 指标 |

## 关键配置（`rl_cli.py`，原样保留）

```ini
RL_MAX_ITERATIONS = 200           # 最大迭代次数（训练流程较长）
DEFAULT_MODEL = "anthropic/claude-opus-4.6"  # 使用强模型指导训练
RL_TOOLSETS = ["terminal", "web", "rl"]    # 可用的工具集
```

## 方法论意义

强制 `rl_test_inference` 是正式训练前的**硬性验证门禁**，与 [[验证门禁化]]（爱奇艺 Specflow Blocker Gate、美团 Pre-PR、vivo 验证门禁化）的跨团队共识同构；训练前小规模实验+正式训练前测试的组合亦呼应 [[双轨校验]] 思想。本流程是 [[research-ready训练闭环]] 的执行骨架，核心算法为 [[grpo算法]]。