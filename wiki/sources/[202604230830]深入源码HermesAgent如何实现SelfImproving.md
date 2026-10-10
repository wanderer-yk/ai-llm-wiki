---
type: source
title: '深入源码：Hermes Agent 如何实现 "Self-Improving"'
tags: [hermes-agent, self-improving, 源码分析, memory, skill, nudge-engine, RDSHermes]
related: [hermes-agent, RDSHermes, 三剑, 千问AI平台, nous-research, openclaw, memory-skill-nudge三子系统自进化闭环, memory容量上限倒逼压缩, 快照冻结与前缀缓存, 声明式事实记忆, memory与skill职责边界, skill自动创建触发条件, skill局部patch修补, skill轻量索引按需加载, 双计数器nudge触发, 领域知识护城河论, 三会话自进化实证案例, 记忆内容威胁模式扫描, skill安全扫描统一门禁, 组织级自进化, 密钥托管凭证隔离, skill生命周期元数据, skill组合成工作流, skill创建透明度, 团队治理写操作二次确认, 用得越久越好用, "sources/[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践"]
authors: [三剑]
year: 2026
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247559661&idx=1&sn=ca9426f948819f172ec44f671127aa29&chksm=e8c148ed1425a7ad80f1e3146f1a458a6f74add0573807ba9c485f69fe52b71849ee58f2e233#rd"
venue: 千问AI平台（微信公众号）
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604230830]深入源码HermesAgent如何实现SelfImproving.html"]
---
# 深入源码：Hermes Agent 如何实现 "Self-Improving"

## 文献信息

- **作者**：[[三剑]]（带原创标记）
- **发布平台**：微信公众号"[[千问AI平台]]"（IP 属地浙江）
- **发布时间**：2026-04-23 08:30
- **分析对象**：[[hermes-agent]]，仓库 `github.com/NousResearch/hermes-agent`（归属 [[nous-research]]）
- **同题异文说明**：本文与 [[sources/[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践|《深度解析：Hermes Agent 如何实现自进化及其 Prompt/Context/Harness 的设计实践》]]（作者飞樰）为**两篇独立文章**——标题、作者、账号、发布日期均不同，已确证不合并，两页互为 `related`。

## 摘要

本文是一篇带精确源码行号引用的 [[hermes-agent]] 自进化（Self-Improving）机制分析。核心结论：Hermes 通过 Memory（"记住你是谁"）+ Skill（"记住怎么做事"）+ Nudge Engine（"保证这个循环不停转"）三个子系统构成自进化闭环，核心主张为"用得越久，越好用"。文章依次覆盖：背景数据与 [[openclaw]] 对比、三子系统总览、Memory 详解、Skill 自动创建/patch/轻量加载、Nudge 双计数器与后台审查、三会话示意案例、安全机制、七条设计取舍、四个展望方向，最后延伸至产品化版本 [[RDSHermes]]（组织级自进化、密钥托管、团队治理）。

## 背景：榜单数据与设计哲学对比

- **榜单数据**（二手数据，无可复验口径）：Hermes Agent 在 OpenRouter 增速 +204%，Top Coding Agents 第一、Top Productivity 第二；上线不到半年 GitHub Star 从 0 到 106k+。
- **设计哲学对比**（作者定性）："一个靠人喂，一个自己长"——[[openclaw]] 的 Skill 靠手写或社区安装，"Agent 本身不会从工作中学到任何东西"；Hermes 能在任务后自动把踩坑经验提炼为可复用 Skill。作者断言 OpenClaw 架构"没有为 Agent 自主学习预留通路"。
- **记忆膨胀对比**：作者称 OpenClaw 的 MEMORY.md 纯追加，数月膨胀成"几万行的怪兽文件"。注意：这与 vivo 文章的 [[追加式上下文]]（会话历史数组 append-only）描述对象不同，引用时勿混淆。

## 三子系统总览

文章总结章节的官方一句话定义："Memory 记住你是谁，Skill 记住怎么做事，Nudge Engine 保证这个循环不停转。"详见 [[memory-skill-nudge三子系统自进化闭环]]。

## Memory：越用越懂你

**双文件结构**：

```bash
~/.hermes/memories/
├── MEMORY.md    # Agent 的个人笔记（环境事实、项目约定、工具怪癖）
└── USER.md      # Agent 对用户的认知（偏好、沟通风格、工作习惯）
```

**MemoryStore 定义**（`tools/memory_tool.py:116-122`）：

```python
# tools/memory_tool.py:116-122
class MemoryStore:
    def __init__(self, memory_char_limit=2200, user_char_limit=1375):
        self.memory_entries: List[str] = [ ]
        self.user_entries: List[str] = [ ]
        self.memory_char_limit = memory_char_limit
        self.user_char_limit = user_char_limit
        self._system_prompt_snapshot: Dict[str, str] = {"memory": "", "user": ""}
```

**超限处理**（`tools/memory_tool.py:248-259`）——不让 `add` 静默成功、也不自动压缩，而是失败并返回全部 `current_entries`，把"淘汰/合并"决策交还模型，构成一次自我反思（详见 [[memory容量上限倒逼压缩]]）：

```python
# tools/memory_tool.py:248-259
if new_total > limit:
    current = self._char_count(target)
    return {
        "success": False,
        "error": (
            f"Memory at {current:,}/{limit:,} chars. "
            f"Adding this entry ({len(content)} chars) would exceed the limit. "
            f"Replace or remove existing entries first."
        ),
        "current_entries": entries,
        "usage": f"{current:,}/{limit:,}",
    }
```

**冻结快照**（`tools/memory_tool.py:124-140`）——会话内系统提示词不变以共享 Prefix Cache，新写入只落盘、下个会话才生效（详见 [[快照冻结与前缀缓存]]）：

```python
# tools/memory_tool.py:124-140
def load_from_disk(self):
    mem_dir = get_memory_dir()
    self.memory_entries = self._read_file(mem_dir / "MEMORY.md")
    self.user_entries = self._read_file(mem_dir / "USER.md")
    # 会话开始时冻结快照，之后不再变动
    self._system_prompt_snapshot = {
        "memory": self._render_block("memory", self.memory_entries),
        "user": self._render_block("user", self.user_entries),
    }
```

**MEMORY_GUIDANCE**（`agent/prompt_builder.py:144-162`，原文以 makefile 高亮渲染，实为 Python 源码）——要求写声明式事实而非命令式指令，记忆价值标准是"减少未来用户干预"（详见 [[声明式事实记忆]]）：

```python
# agent/prompt_builder.py:144-162
MEMORY_GUIDANCE = (
    "You have persistent memory across sessions. Save durable facts using the memory "
    "tool: user preferences, environment details, tool quirks, and stable conventions.\n"
    "Prioritize what reduces future user steering — the most valuable memory is one "
    "that prevents the user from having to correct or remind you again.\n"
    "Write memories as declarative facts, not instructions to yourself. "
    "'User prefers concise responses' ✓ — 'Always respond concisely' ✗. "
    "'Project uses pytest with xdist' ✓ — 'Run tests with pytest -n 4' ✗."
)
```

**Memory 与 Skill 职责边界**：Tool Schema 边界规则 "If you've discovered a new way to do something, save it as a skill"——Memory 存事实、Skill 存操作步骤（详见 [[memory与skill职责边界]]）。

## Skill：越用越会做事

**目录结构**：

```text
~/.hermes/skills/
├── devops/
│   └── flask-k8s-deploy/
│       ├── SKILL.md          # 主指令
│       ├── references/       # 参考文档
│       └── templates/        # 模板文件
└── software-development/
    └── fix-pytest-fixtures/
        └── SKILL.md
```

**SKILL.md 典型结构**（以 `flask-k8s-deploy` 为例）：YAML frontmatter（`name`/`description`/`version`）+ "When to use"（触发场景）+ "Steps"（步骤）+ "Pitfalls"（踩坑记录）。关键声明：**Pitfalls 不是预先写好的，而是 Agent 踩坑后追加的**——Skill 层面 self-improving 的直接体现。

**自动创建触发**：`skill_manage` 工具 schema（`tools/skill_manager_tool.py:681-701`）内置创建/更新/patch 触发条件，Agent 无需用户指示即可自主沉淀 Skill（详见 [[skill自动创建触发条件]]）。系统提示词含维护责任感提示："Skills that aren't maintained become liabilities"。

**局部 patch 修补**：`_patch_skill`（`tools/skill_manager_tool.py:397-485`）用 `fuzzy_find_and_replace` 模糊匹配 + 修改后安全扫描 + 失败自动回滚 + 原子写入，非全量重写（详见 [[skill局部patch修补]]）。

**轻量索引按需加载**：系统提示词只放轻量索引（Skill 名 + 一句话描述），相关时才 `skill_view` 加载全文——作者称之为"动态图书馆"，对照其给 OpenClaw 的"重型背包"定性（SOUL.md/IDENTITY.md 全塞上下文，Token 浪费与注意力稀释）（详见 [[skill轻量索引按需加载]]）。

## Nudge Engine：保证循环不停转

**双计数器**：Memory 按用户回合计（信息源于用户输入，`run_agent.py:1328-1331`）、Skill 按迭代计（经验源于工具使用过程，`run_agent.py:1428-1431`），阈值默认均为 10（Skill 侧可配置）；主动调用 `memory`/`skill_manage` 则重置（详见 [[双计数器nudge触发]]）。

**后台 fork 审查**（`run_agent.py:2665-2711`，`_spawn_background_review`）：触发后在后台 fork 独立 review agent——输出重定向 /dev/null、max_iterations=8、禁用自身 nudge 防无限递归、与主 agent 共享 `_memory_store`、daemon 线程；审查提示词以 "Nothing to save." 收尾防"交差式"写入；审查在响应发送给用户之后才触发。设计理由："自省不应占用用户任务的 attention budget"（该证据应补入既有页 [[后台审查agent]]）。

## 三次会话案例（示意性叙事，非实测）

**⚠️ 本文明确为场景演绎**，不得当作实测性能声明引用（详见 [[三会话自进化实证案例]]）。

| 维度 | 会话 1 (冷启动) | 会话 2 (Skill 复用) | 会话 3 (全协同) |
|------|----------------|--------------------|----------------|
| 工具调用 | 12 次 | 9 次 | 6 次 |
| 错误数 | 2 | 1 | 0 |
| Memory | 无 | 触发写入 | 系统提示词注入 |
| Skill | 触发创建 | 复用 + 自我修补 | 复用已修补版本 |

会话 1 踩 ImagePullBackOff、CrashLoopBackOff 两坑，Review Agent 创建 Skill 的完整调用：

```python
skill_manage(action="create", name="flask-k8s-deploy", category="devops",
    content="""
    ---
    name: flask-k8s-deploy
    description: Deploy a Flask app to Kubernetes with health checks
    ---
    ## Steps
    1. Create Dockerfile with gunicorn
    2. Build and push image to registry BEFORE kubectl apply
    3. Write deployment.yaml with livenessProbe → /health
    ## Pitfalls
    - MUST push image to registry first, otherwise ImagePullBackOff
    - Flask 默认没有 /health 端点，需手动添加
    - livenessProbe path 必须返回 200
    """)
```

会话 2（Django 部署）迭代日志——Skill 复用 + 遭遇 Skill 未覆盖的 DisallowedHost 错误后自我修补：

```
iter 1:  skill_view("flask-k8s-deploy")   → 加载完整 Skill
iter 2:  read_file("manage.py")           → 确认 Django 项目结构
iter 3:  write_file("Dockerfile")         → 用 gunicorn（Skill 指示）
iter 4:  添加 /health 端点（Skill Pitfalls 提醒）
iter 5:  terminal("docker build && docker push")
         → 先 push 再 apply（Skill Steps 第 2 步）
iter 6:  write_file("deployment.yaml")    → livenessProbe → /health
iter 7:  terminal("kubectl apply")
         → 💥 DisallowedHost 错误！Django 特有的问题，Skill 没覆盖
iter 8:  修改 deployment.yaml 添加 ALLOWED_HOSTS env
iter 9:  terminal("kubectl apply")        → ✅ 成功
```

会话 2 的 Review Agent 三动作：写入用户画像 + 记住 registry 地址 + patch Skill 补 ALLOWED_HOSTS 坑——Memory（事实）与 Skill（步骤）双通道同时进化的最小完整闭环。会话 3（FastAPI 部署）依赖三重积累（用户画像 + registry/集群地址 + 已修补 Skill）实现 6 次调用零错误。

## 安全机制

**Memory 内容威胁模式扫描**（详见 [[记忆内容威胁模式扫描]]）——设计理由：Memory 注入系统提示词，被诱导记住 "ignore all previous instructions" 即等于下个会话被劫持：

```python
# tools/memory_tool.py:65-81
_MEMORY_THREAT_PATTERNS = [
    (r'ignore\s+(previous|all|above|prior)\s+instructions', "prompt_injection"),
    (r'do\s+not\s+tell\s+the\s+user', "deception_hide"),
    (r'system\s+prompt\s+override', "sys_prompt_override"),
    (r'curl\s+[^\n]*\$\{?\w*(KEY|TOKEN|SECRET|PASSWORD)', "exfil_curl"),
    ...
]
```

**Skill 安全扫描统一门禁**（详见 [[skill安全扫描统一门禁]]）——Agent 自创 Skill 与 Hub 安装 Skill 走同一套检查：

```python
# tools/skill_manager_tool.py:56-74
def _security_scan_skill(skill_dir):
    result = scan_skill(skill_dir, source="agent-created")
    allowed, reason = should_allow_install(result)
    if allowed is False:
        report = format_scan_report(result)
        return f"Security scan blocked this skill ({reason}):\n{report}"
```

**RDSHermes 密钥托管**（厂商自述，详见 [[密钥托管凭证隔离]]）：AK/SK 由网关代理鉴权，密钥不落盘、不暴露给 Agent 也不暴露给用户。

## 设计取舍一览（七条）

| 设计决策 | 表面效果 | 背后的考量 |
|---------|---------|-----------|
| Memory 限 2200 chars | 迫使 Agent 挑重要的记 | 低质量 Memory 注入系统提示词 = 每次 API 调用都带噪声 |
| 声明式事实 vs 操作步骤分离 | Memory 存事实，Skill 存步骤 | 两者的更新频率、触发条件、安全风险完全不同 |
| 冻结快照模式 | 系统提示词会话内不变 | 保护前缀缓存，避免每轮 API 调用重新计费 |
| 后台 fork 审查 | 用户感知不到 review 过程 | 自省不应占用用户任务的 attention budget |
| Nudge 计数器可配置 | 默认 10 | 太频繁浪费 API 成本，太稀疏错过学习机会 |
| patch 优先于全量重写 | 局部修复 Skill | 保留已验证的稳定部分，只改需要改的 |
| 安全扫描 + 自动回滚 | 拒绝恶意写入 | Memory/Skill 最终进入系统提示词，是一等安全边界 |

## Skill 自动进化的下一步（⚠️ 作者展望，非已实现特性）

1. **生命周期管理**（[[skill生命周期元数据]]）：frontmatter 现仅 `name`/`description`/`version`；提议增加 `last_used`/`use_count`/`success_rate` 实现自动降权、归档、过时检测。
2. **技能组合**（[[skill组合成工作流]]）：高频共现 Skill 自动合成工作流（`flask-k8s-deploy` + `nginx-reverse-proxy` → `full-stack-deploy`），从"记住"到"思考"。
3. **创建透明度**（[[skill创建透明度]]）：当前创建静默，改进为创建后通知供用户审核纠正。
4. **团队治理**（[[团队治理写操作二次确认]]）：写操作二次确认 + 会话可追溯可审计（RDSHermes 已做，厂商自述）。

## RDSHermes：从"开发者工具"到"团队都能用"（⚠️ 厂商自述，未经第三方验证）

详见 [[RDSHermes]]。定位："开源 Hermes 是给开发者的引擎，RDSHermes 是给整个团队的成品车"；"不是所有人都会写 `config.yaml`，但所有人都会打字"。

| | 开源 Hermes Agent | RDSHermes |
|------|------------------|-----------|
| 开始使用 | 命令行安装，手写 config.yaml | 控制台一键开通，零配置 |
| 对话界面 | 终端 CLI | 内置 WebUI，打开浏览器就能对话 |
| 接入 IM | 内置 Gateway，config.yaml 配凭证后命令行启动 | 控制台里填个 App ID 就完成 |
| 数据库连接 | 手动配连接串，密码明文写配置 | 一键接入 RDS 实例，密码自动加密 |
| 云凭证管理 | AK/SK 写进环境变量或配置文件 | 加密托管，网关代理鉴权，密钥不落盘 |
| 技能管理 | Agent 自动创建，磁盘文件 | Skill Hub 预装专业技能 |

- **四件套**：数据库安全纳管（MySQL/PostgreSQL/SQL Server/MariaDB 一键接入、密码提交瞬间加密、可设只读模式"Agent 能查但不能改"）+ 身份认证托管（AK/SK 网关代理鉴权）+ 内置数据库专业技能（Skill Hub 预装智能巡检/慢 SQL 诊断/索引优化，解决冷启动）+ 全链路监控审计（Token 消耗可监控、安全事件有告警）。
- **组织级自进化**（[[组织级自进化]]）：Skill 云端存储，一个 DBA 踩过的坑全团队 Agent 绕过。
- **上线与迁移**：已上线阿里云 RDS AI 应用市场（免费试用）；`hermes claw migrate` 一条命令从 OpenClaw/RDSClaw 导入全部配置和记忆数据。
- **效果声明**（无测量口径）：DBA 飞书群 @一下晨间巡检从 40 分钟缩短到 2 分钟；市场部同事 WebUI 一句话查渠道数据；开发者排查不等 DBA 排期。

## 总结

- **三子系统定论**："Memory 记住你是谁，Skill 记住怎么做事，Nudge Engine 保证这个循环不停转。"
- **版本与生态事实**（作者陈述）：Hermes v0.6.0 之前"只能跑单 Agent"；Profiles 补多实例；MCP Server Mode 打通 IDE 生态；迁移工具覆盖 sessions/cron/memory，"OpenClaw 用户的切换门槛已经被系统性地拆掉了"。
- **OpenClaw "历史使命"论**（⚠️ 作者单方定性）："需要调教指南的工具、升级就崩溃的系统、越用记忆文件越大越慢的架构——正在完成自己的历史使命。"
- **核心价值主张**（[[用得越久越好用]]）："用得越久，越好用"是 Hermes 相对 OpenClaw 的架构级差异。

## 源码行号证据索引

| 文件:行号 | 内容 |
|---|---|
| `tools/memory_tool.py:65-81` | `_MEMORY_THREAT_PATTERNS` 威胁正则 |
| `tools/memory_tool.py:116-122` | MemoryStore 定义（2200/1375 字符上限） |
| `tools/memory_tool.py:124-140` | `load_from_disk` 冻结快照 |
| `tools/memory_tool.py:248-259` | 超限处理 |
| `agent/prompt_builder.py:144-162` | MEMORY_GUIDANCE 提示词 |
| `tools/skill_manager_tool.py:56-74` | `_security_scan_skill` 统一安全门禁 |
| `tools/skill_manager_tool.py:397-485` | `_patch_skill` 局部修补 |
| `tools/skill_manager_tool.py:681-701` | SKILL_MANAGE_SCHEMA 触发条件 |
| `run_agent.py:1328-1331` | Memory 计数器 |
| `run_agent.py:1428-1431` | Skill 计数器 |
| `run_agent.py:2665-2711` | `_spawn_background_review` 后台审查 |

数据目录：`~/.hermes/`、`~/.hermes/memories/`、`~/.hermes/skills/`。

## 归因与可信度注意事项

1. "12→9→6 调用、2→1→0 错误"为叙事性示意案例，非实测性能数据。
2. 作者对 [[openclaw]] 的全部批评（无学习通路、重型背包、三宗罪、历史使命论）为单方定性，引用必须归因于作者。
3. [[RDSHermes]] 全部能力与效果数据（四件套、只读模式、密钥托管、40min→2min）为厂商侧自述，未经第三方验证。
4. "生命周期管理""技能组合""创建透明度"为作者展望（"如果能……"句式），非 Hermes 已实现特性，与 `_patch_skill` 等源码级已实现机制须严格区分。
5. +204% / 106k Star 等榜单数据无测量口径。

## 相关页面

- [[hermes-agent]] · [[RDSHermes]] · [[nous-research]] · [[openclaw]]
- 核心机制：[[memory-skill-nudge三子系统自进化闭环]] · [[memory容量上限倒逼压缩]] · [[快照冻结与前缀缓存]] · [[声明式事实记忆]] · [[memory与skill职责边界]] · [[skill自动创建触发条件]] · [[skill局部patch修补]] · [[skill轻量索引按需加载]] · [[双计数器nudge触发]] · [[后台审查agent]]
- 安全与治理：[[记忆内容威胁模式扫描]] · [[skill安全扫描统一门禁]] · [[组织级自进化]] · [[密钥托管凭证隔离]] · [[团队治理写操作二次确认]]
- 展望与主张：[[skill生命周期元数据]] · [[skill组合成工作流]] · [[skill创建透明度]] · [[领域知识护城河论]] · [[用得越久越好用]] · [[三会话自进化实证案例]]
- 同主题另一来源：[[sources/[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践]]