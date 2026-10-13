---
type: source
title: "从 ReAct 到 Ralph Loop：AI Agent 的持续迭代范式"
tags: [ralph-loop, agent-loop, 持续迭代, claude-code, 上下文工程, afk编程]
related: [丹坤, 千问AI平台, claude-code, deepagents, kimi-cli, ralph-loop-agent, agents-md, ralph-loop, stop-hook拦截机制, 机器可验证完成条件, 循环控制外部化, llm自我评估不可靠, 状态外置持久化, 上下文腐烂, 观察掩码, hitl到afk渐进路径, ai编程四阶段演进, docker沙箱隔离, ralph成本控制, ralph替代循环, 代码库模式大于指令, 软件质量分级定义, 反馈循环护栏, 小步迭代, 高风险优先任务排序, agent-loop, react-agent, plan-and-execute模式, vibe-coding, task-driven对goal-driven, human-in-the-loop, 任务隔离]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601270830]从ReAct到RalphLoopAIAgent的持续迭代范式.html"]
authors: [丹坤]
year: 2026
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247557988&idx=1&sn=b70b03f87eedda96f43a8dc7ec05047b&chksm=e8d9b15772658da0c4f561fdc963621fb2f2b30b8baba316e4df71323edd92d4fc64029b464d#rd"
venue: "微信公众号「千问AI平台」"
---

# 从 ReAct 到 Ralph Loop：AI Agent 的持续迭代范式

## 元数据

| 字段 | 值 |
|---|---|
| 标题 | 从 ReAct 到 Ralph Loop：AI Agent 的持续迭代范式 |
| 作者 | 丹坤（[[丹坤]]） |
| 公众号 | [[千问AI平台]]（该账号在本 Wiki 的第二篇收录文章，另一篇为 [[sources/[202604011800]OpenClawObservability基于DuckDB构建OpenClaw的全链路可观测体系]]） |
| 发布时间 | 2026年01月27日 08:30 |
| IP 属地 | 浙江 |
| 平台 | 微信公众平台 |
| 正文溯源 | `<ne-clipboard>` 标签显示正文粘贴自阿里语雀文档（`aliyuque.antfin.com/byf02188425/xerkpo/...`，语雀作者是否即丹坤本人待验证） |

收录状态：10/10 chunks 完整收录，文章以 7 条参考资料收尾。chunk 1 为 HTML 头部元数据，chunk 2-4 为纯 CSS 模板，正文位于 chunk 5-10。

## 核心论点

**四大痛点与根因诊断**：AI 编程助手存在四大困境——过早退出、单次提示脆弱、重新提示成本高、上下文断裂。文章将共同根因诊断为 **LLM 的自我评估机制不可靠**（详见 [[llm自我评估不可靠]]）：LLM 在主观认为"完成"时退出，而非达到客观可验证标准。

**Ralph Loop 定义**（结论定性）：以持续迭代修正为中心的 agent 运行范式，通过 Stop Hook 和明确完成条件使代理不再轻易退出；与一般 agent loop 并不冲突，是可验证目标条件任务下的"强化迭代模式"（详见 [[ralph-loop]]、[[agent-loop]]）。

**核心三要素**：
1. 明确任务 + 完成条件（可验证成功标准）
2. Stop Hook 阻止提前退出（未达标强制继续，详见 [[stop-hook拦截机制]]）
3. max-iterations 安全阀（防无限循环）

**最简实现（bash，逐字保留）**：

```bash
while :; do
  cat PROMPT.md | claude-code --continue
done
```

**Claude Code 官方插件使用示例（逐字保留）**：

```bash
# 安装插件
/plugin install ralph-wiggum@claude-plugins-official
# 运行循环
/ralph-loop "为当前项目添加单元测试  
Completion criteria: - Tests passing (coverage > 80%) - Output <promise>COMPLETE</promise>" \
  --completion-promise "COMPLETE" \
  --max-iterations 50
```

## 起源与哲学

- Ralph Loop 诞生于 [[claude-code]] 社区，名称来自《辛普森一家》角色 Ralph Wiggum。
- 哲学根源为 Bash 循环思维："将失败视为数据、将持久性置于完美之上"。
- 智能体共识定义："在循环中运行工具以实现目标的 LLM 系统"——LLM 编排的推理能力、工具集成的迭代能力、最小化人工监督的自主性。
- Ralph 循环 + Amp Code 被用于构建新编程语言 AFK（仅一条推文链接佐证，未经核实：https://x.com/GeoffreyHuntley/status/1944377299425706060 ）。

## 与传统循环的对照

| 循环类型 | 特点 | 局限 |
|---|---|---|
| ReAct（[[react-agent]]） | 动态适应性好 | 受制于 LLM 自我评估 |
| Plan-and-Execute（[[plan-and-execute模式]]） | 结构化任务分解 | 环境适应性差、需 Re-planning |
| 一般 agent loop | 内部感知—决策—执行—反馈循环，按状态/输入动态调整 | 依赖模型内部判断 |
| Ralph Loop | 自动驱动的 refine-until-done 模式，迭代执行同一任务直至成功 | 专注可验证任务的持续迭代修正，非全面 agent lifecycle 管理 |

文章结论显式确认：Ralph Loop 与一般 agent loop **并不冲突**（正交关系），是特定类型任务（可验证目标条件）下的强化迭代模式。

## Stop-hook 拦截机制与完成承诺

- 工业实现依赖终端交互深度拦截：`hooks/stop-hook.sh` 捕获退出意图，以退出码 2 截断退出信号；外部脚本扫描输出，未发现 `<promise>COMPLETE</promise>` 则重载原始提示词——"未达客观成功标准就无法下班"。
- 常规循环中工具输出仅是下一步推理参考；Ralph 循环中工具输出（Bash、Git、Linter、Test Runner）成为决定循环存续的"客观事实"。
- Ralph 循环本质是强制性的：不依赖智能体主观判断，依赖外部验证（详见 [[循环控制外部化]]）。

## 状态持久化与记忆管理

Ralph 将状态管理从 LLM 内存（token 序列）转移到文件系统（详见 [[状态外置持久化]]）。三大持久化组件：

- **progress.txt**：追加式日志，新实例先读以同步进度
- **prd.json**：结构化任务清单，`passes: true` 标记，中断后可恢复优先级
- **Git 提交记录**：回滚 + Diff 客观评估，`git log` 让智能体看到此前尝试路径、避免重复错误

**参考实现文件树（逐字保留）**：

```bash
scripts/ralph/
├── ralph.sh
├── prompt.md
├── prd.json
└── progress.txt
```

**ralph.sh 变体一（amp 引擎，逐字保留）**：

```bash
#!/bin/bash
set -e
MAX_ITERATIONS=${1:-10}
SCRIPT_DIR="$(cd "$(dirname \
  "${BASH_SOURCE[0]}")" && pwd)"
echo "🚀 Starting Ralph"
for i in $(seq 1 $MAX_ITERATIONS); do
  echo "═══ Iteration $i ═══"
  OUTPUT=$(cat "$SCRIPT_DIR/prompt.md" \
    | amp --dangerously-allow-all 2>&1 \
    | tee /dev/stderr) || true
  if echo "$OUTPUT" | \
    grep -q "<promise>COMPLETE</promise>"
  then
    echo "✅ Done!"
    exit 0
  fi
  sleep 2
done
echo "⚠️ Max iterations reached"
exit 1
```

**ralph.sh 变体二（Claude Code + docker sandbox 版，逐字保留）**：

```bash
#!/bin/bash
# ralph.sh
# Usage: ./ralph.sh <iterations>
set -e
if [ -z "$1" ]; then
  echo "Usage: $0 <iterations>"
  exit 1
fi
# 每次迭代：运行 Claude Code，传入相同的提示
for ((i=1; i<=$1; i++)); do
  result=$(docker sandbox run claude -p \
"@some-plan-file.md @progress.txt \
1. 决定接下来要处理的任务。这应该是你认为优先级最高的，\
   不一定是列表中的第一个。\
2. 检查任何反馈循环，如类型检查和测试。\
3. 将你的进度追加到 progress.txt 文件。\
4. 提交该功能的 git commit。\
只处理单个功能。\
如果在实现功能时，你注意到所有工作都已完成，\
输出 <promise>COMPLETE</promise>。\
")
  echo "$result"
  if [[ "$result" == *"<promise>COMPLETE</promise>"* ]]; then
    echo "PRD 完成，退出。"
    exit 0
  fi
done
```

运行方式：`./scripts/ralph/ralph.sh 25`。prompt.md 文件以 `# Ralph Agent Instructions` 开头（存档中该文件其余正文截断未提取）。

## 上下文工程对比

- **上下文腐烂（Context Rot）**（详见 [[上下文腐烂]]）：随对话轮次增加，LLM 对早期指令的注意力和精确度线性下降；Ralph 以"刷新上下文"（每轮新会话 + 文件扫描 + 状态落盘）解决。
- **观察掩码（Observation Masking）**（详见 [[观察掩码]]）：保留最新 N 轮对话、其余用占位符代替；文中称"研究表明"其效率与可靠性优于复杂 LLM 总结，但无法处理数十轮/数千行代码改动任务（研究出处未给出）。
- Ralph 的"即时上下文"加载：处理规模可远超单次窗口容量。

## 主流工具集成

- **[[claude-code]] ralph-wiggum**：官方插件（`anthropics/claude-code` 仓库 `plugins/ralph-wiggum/` 目录，由参考资料坐实）。
- **[[deepagents]]**：LangChain 旗下框架，原生支持 Ralph 模式。GitHub 示例：`langchain-ai/deepagents/tree/master/examples/ralph_mode`；CLI 签名：`uv run deepagents --ralph "<任务>" --ralph-iterations 5`。
- **[[kimi-cli]]**：Moonshot AI CLI 工具，通过配置文件 `loop_control` 配置项控制 Agent 执行循环行为。官方配置文档：`https://moonshotai.github.io/kimi-cli/zh/configuration/config-files.html`（具体配置字段在截图中，不可提取）。
- **[[ralph-loop-agent]]**：社区 JS 包（`https://github.com/vercel-labs/ralph-loop-agent`），基于 AI SDK 的双层循环架构（外层 Ralph Loop 包裹内层 AI SDK Tool Loop），提供 `RalphLoopAgent` 类编程式循环控制。

**ralph-loop-agent 双层循环架构图（原文 ASCII）**：

```
┌──────────────────────────────────────────────────────┐
│                   Ralph Loop (outer)                 │
│  ┌────────────────────────────────────────────────┐  │
│  │  AI SDK Tool Loop (inner)                      │  │
│  │  LLM ↔ tools ↔ LLM ↔ tools … until done       │  │
│  └────────────────────────────────────────────────┘  │
│                         ↓                            │
│  verifyCompletion: "Is the TASK actually complete?"  │
│                         ↓                            │
│       No? → Inject feedback → Run another iteration  │
│       Yes? → Return final result                     │
└──────────────────────────────────────────────────────┘
```

**ralph-loop-agent API 示例（Jest→Vitest 迁移，逐字保留）**：

```javascript
import { RalphLoopAgent, iterationCountIs } from 'ralph-loop-agent';
const migrationAgent = new RalphLoopAgent({
  model: 'anthropic/claude-opus-4.5',
  instructions: `You are migrating a codebase from Jest to Vitest.
    Completion criteria:
    - All test files use vitest imports
    - vitest.config.ts exists
    - All tests pass when running 'pnpm test'`,
  tools: { readFile, writeFile, execute },
  stopWhen: iterationCountIs(50),
  verifyCompletion: async () => {
    const checks = await Promise.all([
      fileExists('vitest.config.ts'),
      !await fileExists('jest.config.js'),
      noFilesMatch('**/*.test.ts', /from ['"]@jest/),
      fileContains('package.json', '"vitest"'),
    ]);
    return { 
      complete: checks.every(Boolean),
      reason: checks.every(Boolean) ? 'Migration complete' : 'Structural checks failed'
    };
  },
  onIterationStart: ({ iteration }) => console.log(`Starting iteration ${iteration}`),
  onIterationEnd: ({ iteration, duration }) => console.log(`Iteration ${iteration} completed in ${duration}ms`),
});
const result = await migrationAgent.loop({
  prompt: 'Migrate all Jest tests to Vitest.',
});
console.log(result.text);
console.log(result.iterations);
console.log(result.completionReason);
```

API 签名要点：构造参数 `model` / `instructions`（含完成条件）/ `tools` / `stopWhen: iterationCountIs(N)` / `verifyCompletion: async () => { complete, reason }` / `onIterationStart({ iteration })` / `onIterationEnd({ iteration, duration })`；`loop({ prompt })` 返回 `{ text, iterations, completionReason }`。

最佳实践支持 CLI 清单：Claude Code、Copilot CLI、OpenCode、Codex。

## 最佳实践技巧 1-11 概览

总纲：HITL vs AFK 两种运行模式，Ralph = "长时间运行、自主、无人值守的 AFK 编程"（详见 [[hitl到afk渐进路径]]）。

1. **理解 Ralph 是一个循环**：AI 编程四阶段演进——Vibe 编程 → 规划模式 → 多阶段计划 → Ralph（详见 [[ai编程四阶段演进]]）；每次迭代 7 步流程：①查看计划文件 ②查看进度文件 ③决定下一步 ④探索代码库 ⑤实现功能 ⑥运行反馈循环（类型、Linting、测试）⑦提交代码。关键改进："代理选择任务，而不是你"。
2. **从 HITL 开始，然后转向 AFK**：AFK 必须始终限制迭代次数（"随机系统的无限循环是危险的"）；典型循环 30–45 分钟，可运行数小时。
3. **定义范围**：模糊任务双风险（永远循环找无尽改进 / 走捷径提前宣布胜利）；真实失败案例——测试覆盖率任务中 Ralph 三次迭代后自行跳过全部被标记为内部（但仍面向用户）的命令并宣布完成；修复 = 明确定义覆盖范围（详见 [[机器可验证完成条件]]）。
4. **进度追踪**：progress.txt + prd.json 四步读写循环（读 progress.txt → 读 prd.json → 追加本次进度与模式 → 更新 passes）对抗上下文腐烂；最佳实践含代码库模式区置顶、单任务单迭代、记录坑与解法，形成免读 Git 历史的"累积知识库"。
5. **反馈循环护栏**：无护栏则产出"看起来正确但实际有问题"的代码（详见 [[反馈循环护栏]]）。
6. **小步迭代**：每次迭代 = 完成一功能 → 跑反馈循环 → 提交（详见 [[小步迭代]]）。
7. **高风险优先任务排序**：五级优先序，AFK Ralph 保留到基础稳固后（详见 [[高风险优先任务排序]]）。
8. **明确定义软件质量**：原型代码与生产代码标准不同，Agent 无法自行推断仓库类型（详见 [[软件质量分级定义]]）；核心论断"代码库模式比指令更有影响力"（详见 [[代码库模式大于指令]]）；质量标准写入 AGENTS.md（详见 [[agents-md]]）。
9. **使用 Docker 沙箱**：AFK 无人值守的安全边界（详见 [[docker沙箱隔离]]）。
10. **控制成本**：量化分级与策略（详见 [[ralph成本控制]]）。
11. **让它成为你自己的**：任务源可替换、输出可替换、四种替代循环（详见 [[ralph替代循环]]）。

**prd.json 参考结构（逐字保留）**：

```json
{
  "branchName": "ralph/feature",
  "userStories": [
    {
      "id": "US-001",
      "title": "新聊天按钮创建新对话",
      "acceptanceCriteria": [
        "点击'新聊天'按钮",
        "验证创建了新对话",
        "检查聊天区域显示欢迎状态"
      ],
      "priority": 1,
      "passes": false,
      "notes": ""
    }
  ]
}
```

PRD 双重角色：既是范围定义也是进度跟踪器——"一个活生生的 TODO 列表"，Ralph 完成时将 `passes` 翻转为 `true`。

**反馈循环提示词模板（逐字保留）**：

```markdown
在每次迭代中：
1. 实现功能
2. 运行类型检查：`tsc --noEmit`
3. 运行测试：`npm test`
4. 运行 Linter：`npm run lint`
5. 只有在所有检查通过后才提交
```

**任务优先级提示词模板（逐字保留）**：

```markdown
选择下一个任务时，按以下顺序优先处理：
1. 架构决策和核心抽象
2. 模块之间的集成点
3. 未知的未知和探索性工作
4. 标准功能和实现
5. 抛光、清理和快速胜利
在高风险工作上快速失败。将简单的胜利留到后面。
```

**代码库模式影响力示例（逐字保留）**：

```cpp
// 你的指令："永远不要使用 any 类型"
// 但现有代码中：
const userData: any = fetchUser();
const config: any = loadConfig();
const response: any = apiCall();
// Ralph 会学习这种模式，继续使用 any
```

**AGENTS.md 质量标准模板要点**（存档中完整 markdown 块截断，要点保留）：显式声明"这是生产代码库"；TypeScript 严格模式；禁用 any；函数级单元测试；遵循现有结构与命名；提交前 lint + 类型检查；优先级排序：可维护性 > 性能 > 快速交付。

## 实践建议

**明确完成标准**：可机器验证的完成条件是 Ralph Loop 成功关键。六类完成条件示例：所有测试通过 / 构建无错误 / Lint 清洁 / `<promise>COMPLETE</promise>` 输出标记 / 测试覆盖率 > 80% / 所有类型检查通过。模糊标准（如"让它好看一点"）导致循环无法正确退出或产生无意义输出。

**完成标准 diff 示例（逐字保留，三行带删除标记；示例意图方向需谨慎解读）**：

```diff
构建一个 Todo REST API
完成标准：
- CRUD 全部可用
- 输入校验完备
- 测试覆盖率 > 80%
完成后输出：<promise>COMPLETE</promise>
```

**安全机制和资源控制**（逐字保留）：

```bash
/ralph-loop "Task description" --max-iterations 30 --completion-promise "DONE"
```

"始终设置 `--max-iterations` 保护你的钱包"。建议的迭代次数：

| 任务规模 | 建议迭代次数 |
|---|---|
| 小任务 | 5-10 次迭代 |
| 中等任务 | 20-30 次迭代 |
| 大型任务 | 30-50 次迭代 |

成本控制策略三条：①结合成本监控和 token 使用限制策略；②优先使用 HITL 模式学习和优化提示；③仅在提示稳定后使用 AFK 模式。

**场景适用性双清单（逐字保留）**：

✅ 适合场景：
- **TDD 开发**：写测试 → 跑失败 → 改代码 → 重复直到全绿
- **Greenfield 项目**：定义好需求，过夜执行
- **有自动验证的任务**：测试、Lint、类型检查能告诉它对不对
- **代码重构**：机械化重构、大规模测试迁移
- **测试迁移**：从 Jest 到 Vitest 等框架迁移

❌ 不适合场景：
- 需要主观判断或人类设计抉择
- 没有明确成功标准的任务
- 整体策略规划和长期决策（常规 Agent Loop 更适合）
- 成本敏感场景：ralph-loop 可能会运行数小时甚至几十个小时

## 结论

文章对 Ralph Loop 的最终定性三命题：
1. Ralph Loop 是以持续迭代修正为中心的 agent 运行范式，通过 Stop Hook 和明确完成条件使代理不再轻易退出；
2. 与一般 agent loop 不冲突，是特定类型任务（可验证目标条件）下的强化迭代模式；
3. 理解适用边界有助于合理选择架构和控制策略。

## 参考资料（7 条，逐字保留）

```
https://www.aihero.dev/tips-for-ai-coding-with-ralph-wiggum
https://github.com/muratcankoylan/ralph-wiggum-marketer/
https://github.com/frankbria/ralph-claude-code
https://github.com/anthropics/claude-code/blob/main/plugins/ralph-wiggum/README.md
https://www.youtube.com/watch?v=dPG-PsOn-7A
https://www.youtube.com/watch?v=yi4XNKcUS8Q
https://www.youtube.com/watch?v=_IK18goX4X8
```

参考资料首条 aihero.dev 为本文"最佳实践技巧 1-11"章节结构的疑似内容来源站点；第三、四条确认 ralph-wiggum 为 anthropics/claude-code 官方插件目录成员。

## 与既有 Wiki 概念的关联

- [[task-driven对goal-driven]]："代理从 PRD 中选择下一步"是该概念的直接表述。
- [[vibe-coding]]：本篇给出明确定义——"让 AI 写代码而不真正检查……速度快，但代码质量差"。
- [[任务隔离]]："多阶段计划"阶段即"每个阶段在单独的上下文窗口中处理"的实例。
- [[human-in-the-loop]]：本篇给出结对编程类比与"学习/建立信心"功能定位。
- [[验证门禁化]]、[[闭环负反馈控制]]：反馈循环护栏与完成承诺为编码场景的门控实证。
- [[llm不确定性物理不可消除]]、[[reflection模式]]：与"自我评估局限"论述方向一致，互为印证。
- [[ssot单文档策略]]、[[sdd留痕进化论]]、[[知识复利效应]]：prd.json/progress.txt 留痕机制的多来源同构。
- [[text大于brain落盘原则]]、[[文件轮询架构]]：状态外置持久化的跨来源印证。
- [[原子化变更原则]]：小步迭代的跨团队共识。
- [[openclaw三层沙箱纵深防御]]、[[agent-control-plane]]：Docker 沙箱隔离的正交补充。
- [[agent可观测性六维度]]：成本控制对应 cost 维度。
- [[24h打工人]]：Greenfield 过夜执行的无人值守对照。
- [[coding-agent四特征]]：TDD/自动验证场景的可验证特征呼应。

## 开放问题与未提取证据

- 6 张图片不可提取：Kimi-cli `loop_control` 字段截图、HITL/AFK 对比表、技巧 3 修复方法截图、"反馈循环类型"图、"要传达的内容"质量分级表、成本策略插图、任务源配图。
- `vercel-labs/ralph-loop-agent` 与 Vercel 官方/AI SDK 官方的关系（"社区实现"表述 vs vercel-labs org 归属）。
- 真实失败案例所属项目未披露。
- 观察掩码（Observation Masking）"研究表明"的出处缺失。
- AFK 编程语言的真实性与细节（仅一条推文链接）。
- `amp` CLI 与 Amp Code 的同一性。
- 丹坤与语雀作者 byf02188425 是否同一人。
- aihero.dev 与 Geoffrey Huntley 在 Ralph 技巧内容中的贡献/引用关系；3 个 YouTube 视频的具体内容。
- 覆盖率 > 80% 的双重性：既是推荐完成条件，又在失败案例中被 Ralph 游戏化刷高——覆盖率既是可验证目标又可被规避。