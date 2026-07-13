---
tags:
  - ai
  - vibe-coding
  - research
created: 2026-06-15
updated: 2026-06-21
---

# Vibe Coding 进阶实战手册

> **从 Coder → Commander，通过自然语言与 AI 对话，让编程从"写代码"转变为"对话式创作"。**
> — Andrej Karpathy


---

## 1. 自检：

| 阶段                                 | 特征                               | 你的位置          |
| ---------------------------------- | -------------------------------- | ------------- |
| **Level 1** 随意对话                   | 想到什么让 AI 做什么，无结构、无记忆             | 👈 **当前**     |
| **Level 2** 结构化工作流                 | 有 PRD、有 CLAUDE.md、分步迭代           | 🎯 **第一阶段目标** |
| **Level 3** 领域知识+技能                | 有 Skills 库、有质量门禁、有 Sub-agents    | 🔮 中期目标       |
| **Level 4** 完整 Agentic Engineering | Hooks + MCP + 自动化闭环 + 多 Agent 协作 | 🌌 远期目标       |

> 你现在在 Level 1。核心任务只有一件事：**用第 2 章的四阶段循环替换"随意对话"习惯。**

---

## 2. 🔥 核心工作流：四阶段循环

这是整个 Vibe Coding 最重要的方法论。**两个顶级教程（SitePoint + EnzeD 终极指南）独立得出完全相同的结论。**

```
PROMPT → GENERATE → REVIEW → REFINE → (循环)
 架构先行   受限批次    AI自审     人工终审
```

### 阶段 1：PROMPT（架构先行）

**在写任何代码之前，强制 AI 先输出架构方案：**

```markdown
我需要构建 [功能]。写代码之前，先生成一个架构规划清单：
- 需要创建/修改哪些文件？
- 哪些是 Server Component，哪些是 Client Component？（说明理由）
- 数据库 Schema 变更？
- 需要哪些环境变量？
- 有哪些安全隐患？

等待我确认后再继续。
```

**Prompt 模板（直接复制使用）：**

```markdown
## Context
- Tech stack: [你的技术栈，如 Node.js 22, React 19, Express 4.x, ES modules]
- 项目当前状态：[一句话描述]

## Feature Request
[你要构建的功能，用用户故事描述]

## Constraints
- TypeScript strict mode, 禁止 `any` 类型
- Zod 校验所有外部输入
- 集中式错误处理
- [项目特定约束]

## Acceptance Criteria
- [可测试的验收标准 1]
- [可测试的验收标准 2]

## Output Format
先用 Plan Mode 输出架构方案，等我确认后再写代码。
```

### 阶段 2：GENERATE（受限批次）

> **50 行 × 4 次 远比 200 行 × 1 次错误少。** AI 输出质量随上下文增长而衰减，小批次错误率比大批次低 3-5 倍。

**关键指令：**
- *"只实现 Phase 1 — 创建 Schema。停。不要开始写 UI。"*
- *"只修改 auth.service.ts，不要碰其他文件。"*
- 每批代码量控制在 50-80 行
- 每个批次完成后必须人工审查

### 阶段 3：REVIEW（AI 自我审计）

每批代码生成后，让 AI 审查自己的产出：

```markdown
审查你刚才写的代码。检查：
- 输入校验是否完整
- 错误处理是否覆盖所有分支
- 依赖版本是否兼容
- 是否存在安全隐患
修复所有发现的问题。
```

> 实测数据：AI 自我审计平均能发现自己代码中的 2-3 个问题。

### 阶段 4：REFINE（人工终审）

你是 **Senior Reviewer**，不是橡皮图章。见第 6 章的完整审查清单。

---

## 3. 📋 CLAUDE.md 实战模板

### 可直接复制的模板

Karpathy 风格（65 行，错误率从 41% → 3%）+ Boris Cherny 原则（≤100 条指令）：

```markdown
# Project: [项目名]
- Tech: [技术栈]
- Entry: [启动命令]

## Rules — ALWAYS
1. 写任何代码前，先读取 memory-bank/architecture.md 和 memory-bank/prd.md
2. 每完成一个主要功能后，更新 memory-bank/architecture.md
3. 强调模块化 — 拆分多个文件，禁止单个文件超过 300 行
4. 每次只实现一个功能，完成并验证后再进入下一个
5. 不确定的事情先问，不要猜
6. 不添加未被要求的功能，50 行能搞定的不写 200 行
7. 只修改必须修改的，绝不顺手重构
8. 所有外部输入用 Zod 校验，禁止 `any` 类型
9. 环境变量和密钥绝不硬编码
10. 先输出架构方案，等确认后再写代码

## Memory Bank
- PRD: memory-bank/prd.md
- Architecture: memory-bank/architecture.md
- Progress: memory-bank/progress.md
- Tech Stack: memory-bank/tech-stack.md
```

### 项目记忆库结构

```
memory-bank/
├── prd.md               # 产品需求文档（用户是谁、痛点在哪、MVP 范围）
├── tech-stack.md         # 技术栈决策及理由
├── architecture.md       # 架构文档（每个里程碑完成后更新）
├── implementation-plan.md # 分步实现计划（只写说明，不写代码）
└── progress.md           # 进度追踪（每完成一步更新）
```

### CLAUDE.md 维护策略

| 信号 | 行动 |
|------|------|
| AI 开始忽略某些规则 | 规则太多了，删减到核心 10 条 |
| AI 行为出现"漂移" | 立即精简，Boris Cherny 建议 ≤100 条 |
| 项目技术栈变更 | 更新 CLAUDE.md 并同步 memory-bank |
| 新对话 AI 不了解项目 | memory-bank 过时了，花 5 分钟更新 |
| 某条规则从不触发 | 删除它，冗余规则是噪音 |

---

## 4. 🎯 Prompt 工程进阶

### 结构化 Prompt 六要素

```markdown
## Context        ← 技术栈、项目现状
## Feature        ← 用用户故事描述需求
## Constraints    ← 技术约束（TypeScript strict、禁止 any、Zod 校验）
## Acceptance     ← 可测试的验收标准
## Output Format  ← 要求先 Plan 还是直接写、文件结构规范
## Examples       ← 好的输出长什么样（可选，但非常有效）
```

### 力度阶梯：控制 AI 思考深度

| 指令 | 效果 | 使用场景 |
|------|------|---------|
| （无特殊指令） | 默认推理 | 简单任务 |
| `think carefully` | 触发更深推理 | 中等复杂度 |
| `think hard` / `ultrathink` | 最强推理 | 架构决策、复杂重构 |

> Claude Code 中优先使用 **Plan Mode**（`Shift+Tab`）而非依赖力度指令。Boris Cherny 本人 80% 时间在 Plan Mode。

### 常见 Prompt 反模式

| ❌ 反模式 | ✅ 正确做法 |
|----------|-----------|
| "帮我做一个博客系统" | "帮我做一个博客系统。先输出架构方案，等我确认。" |
| "这里有个 bug，修一下" | "点击登录按钮后页面白屏，控制台报错：`[粘贴错误]`" |
| "优化这段代码" | "这段代码在 1000 条数据时响应超过 2 秒，优化到 200ms 以内" |
| 一次性塞 500 行需求 | 拆成 3-5 个独立功能，逐个实现 |
| 不提供上下文 | 始终包含技术栈、项目现状、约束条件 |

---

## 5. 🧩 Skills 体系搭建

### Skill 的 description 怎么写

`description` 字段**不是给人看的摘要，是给 AI 看的触发器**：

```yaml
# ❌ 错误（给人看的）
description: "帮助生成 PRD 文档"

# ✅ 正确（给 AI 看的 — 告诉它何时触发）
description: "当用户需要创建产品需求文档、定义 MVP 范围、或从想法转化为结构化需求时使用"
```

### 可复用的 Skill 模板

**Skill 1: PRD 生成器**

```markdown
# PRD Generator

当用户描述一个产品想法时：
1. 先问灵魂三问：用户是谁？痛点在哪？为什么用你的产品？
2. 定义 MVP 范围（P0/P1/P2 优先级）
3. 输出结构化 PRD：用户故事 → 功能列表 → 验收标准 → 不做什么
4. 写入 memory-bank/prd.md
```

**Skill 2: 代码审查员**

```markdown
# Code Reviewer

审查生成的代码，检查以下 5 类高频错误：
1. "全是 Client" — 数据获取组件误标 'use client'
2. 沉默 Catch — catch 块无用户反馈
3. N+1 查询 — .map() 内数据库查询
4. 硬编码密钥 — API Key 在代码中
5. 缺少校验 — 用户输入直达数据库

输出：问题列表 + 修复建议 + 修复后的代码
```

### Skills 组合模式

```
Command（用户触发）
  → Agent（编排调度）
    → Skill A（PRD 生成）
    → Skill B（技术设计）
    → Skill C（代码生成）
    → Skill D（代码审查）
    → Skill E（测试生成）
```

---

## 6. 🔍 AI 代码审查清单

每批 AI 生成的代码，人工走一遍这 5 类最高频错误：

| # | 问题 | 表现 | 修复 Prompt |
|---|------|------|-----------|
| 1 | **"全是 Client"** | 数据获取组件也加了 `'use client'`，本应是 Server Component | "这个组件只做数据获取，改为 Server Component" |
| 2 | **沉默 Catch** | `catch(e) { console.log(e) }` — 用户看到的是坏掉的 UI | "每个 catch 块添加用户可见的错误提示" |
| 3 | **N+1 查询** | `.map()` 内逐条查数据库，100 条数据 = 101 次查询 | "用 `.in()` 或 `Promise.all()` 批量获取" |
| 4 | **硬编码密钥** | API Key 写在代码里 | "所有密钥移到环境变量，添加到 .env.example" |
| 5 | **缺少校验** | 用户输入直达数据库/API | "所有外部输入用 Zod schema 校验" |

---

## 7. 🛠️ Claude Code 进阶技巧

### 精选 10 个最实用的技巧

| # | 技巧 | 怎么做 |
|---|------|--------|
| 1 | **Plan Mode 优先** | `Shift+Tab` 进入只读规划，确认方案后再写代码 |
| 2 | **代码库对话** | `@filename.ts` 引用文件让 AI 理解上下文 |
| 3 | **回退操作** | `/rewind` 回退到之前状态，`Esc-Esc` 撤销 |
| 4 | **后台任务** | `/tasks` 让长时间任务在后台运行 |
| 5 | **项目规则** | `CLAUDE.md` 设定全局规则，`/memory` 设定会话规则 |
| 6 | **Sub-agents** | `.claude/agents/` 定义专业子智能体并行处理 |
| 7 | **Skills** | `.claude/skills/<name>/SKILL.md` 封装可复用能力 |
| 8 | **Hooks** | `.claude/hooks/` 事件触发自动化（PreToolUse 防危险操作） |
| 9 | **切换模型** | `/model` 在 Opus/Sonnet/Haiku 间切换，按任务选模型 |
| 10 | **控制深度** | `/effort` 控制 AI 推理投入程度 |

### 上下文管理黄金法则

| 原则 | 做法 |
|------|------|
| 保持上下文使用率 **< 50-60%** | 接近上限时 AI 质量明显下降 |
| 每个大功能 **换新对话** | 上下文污染是 Vibe Coding 头号敌人 |
| 新对话第一步：**让 AI 读 memory-bank** | "先读取 memory-bank/ 下所有文件，了解项目现状" |
| 并行会话 **不同时操作同一文件** | 架构讨论和文档编写可并行，代码编写不可 |

### Sub-agents 最佳用法

```
主会话（决策层）           Sub-agent 1（搜索/研究）
      │                        │
      ├── "调查 X 方案" ──────→ 只读、搜索、总结
      │←── 研究报告 ──────────
      │                        Sub-agent 2（代码审查）
      ├── "审查 Y 模块" ──────→ 只读、检查、报告
      │←── 审查结果 ──────────
      │                        
      ├── 你做决策，写代码
```

---

## 8. 🚦 质量门禁体系

### 2026 生产级 6 条铁律

| # | 铁律 | 实现方式 |
|---|------|---------|
| 1 | **Always loop** | Agent 在 write→test→run→fix 循环中工作，不单次生成 |
| 2 | **Always test** | 新行为必须有测试，用 `make test` 自动化 |
| 3 | **Always scan** | lint + type-check + security scan，问题 block merge |
| 4 | **Always human-review** | auth / payments / PII / production data → 必须人工审查 |
| 5 | **Always pin** | 锁定依赖版本，审计新增导入 |
| 6 | **Always log** | 维护 AI 操作的审计轨迹（Prompt → Output → Decision） |

### 三层自动化架构

解决 AI 的**失忆**（忘记项目规则）和**偷懒**（走捷径跳过步骤）：

```
机械层: Makefile     →  make test / make lint / make docs（AI 忘不了，因为是硬指令）
记忆层: CLAUDE.md    →  持久项目上下文，每次会话自动加载（像"纹身"）
推理层: Skills       →  将抽象工作流拆解为具体步骤，阻止 AI 走捷径（像"本能"）
```

### Hooks 配置示例

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [{
          "type": "command",
          "command": "echo '检查：不编辑 .env / package-lock.json / 不执行 rm -rf'"
        }]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [{
          "type": "command",
          "command": "npx prettier --write ${CLAUDE_MODIFIED_FILES}"
        }]
      }
    ],
    "Stop": [
      {
        "hooks": [{
          "type": "command",
          "command": "echo '本次修改文件：' && git diff --name-only"
        }]
      }
    ]
  }
}
```

---

## 9. 🗺️ 学习路径：Level 1 → Level 3

> 不用急着到 Level 4。**Level 1→2 是质变，Level 2→3 是量变。** 先把第一阶段练成肌肉记忆。

### 🟢 阶段一：从随意对话到结构化（第 1-3 周）— L1→L2

这是最关键的一步。目标不是学新知识，是**改习惯**。

- [ ] **第 1 周：强制走 PROMPT 阶段** — 每次让 AI 写代码前，先用第 2 章的模板发一段需求，**等 AI 输出架构方案，确认后再让它写**。哪怕只写 20 行代码也走这个流程。
- [ ] **第 2 周：引入 GENERATE 限制** — 学会说"只做这一步，停"。练习把大需求拆成 3-5 个小步骤，每次只让 AI 做一步。
- [ ] **第 3 周：加入 REVIEW 环节** — 每批代码生成后，让 AI 用第 2 章的 Prompt 审查自己的代码。你会发现它真能找出问题。
- [ ] 第 3 周末：建第一个 `memory-bank/`，至少写 `prd.md` 和 `progress.md`
- **里程碑**：能自然说出"先出方案，等我确认"而不再需要刻意提醒自己

### 🟡 阶段二：建立你的系统（第 4-8 周）— L2→L3

- [ ] **CLAUDE.md**：用第 3 章的模板为当前项目写一份，从 5 条 Always 规则起步
- [ ] **审查清单肌肉记忆**：每次 AI 生成代码后，对照第 6 章的 5 类错误走一遍（前 10 次刻意做，之后会变本能）
- [ ] **Prompt 模板库**：把第 2、4 章的模板存好，每次复制使用，逐步按自己项目定制
- [ ] **第一个 Skill**：先只做一个（推荐"代码审查员"），跑通创建→触发→使用的完整流程
- **里程碑**：新项目 10 分钟内搭好 CLAUDE.md + memory-bank，开始用四阶段循环

### 🔵 阶段三：自动化与质量门禁（第 9-12 周）— 巩固 L3

- [ ] 配置 Hooks：至少做 PreToolUse（防危险操作）和 PostToolUse（自动格式化）
- [ ] 建立个人 Skills 库（3-5 个）：PRD 生成器 / 代码审查员 / 测试生成器 / 技术设计
- [ ] `make test` 成为硬门禁 — AI 生成代码 → 跑测试 → 不通过不给过
- [ ] 上下文管理习惯：大功能换新对话，新对话第一步读 memory-bank
- **里程碑**：一次对话产出的代码质量接近自己手写水平，review 只改细节不改逻辑

---

## 10. 📚 核心参考资源

### 必读仓库

| 仓库 | 用途 |
|------|------|
| [[claude-code-best-practice]](https://github.com/shanraisshan/claude-code-best-practice) | ⭐57k Claude Code 圣经，from vibe coding to agentic engineering |
| [[vibe-coding]](https://github.com/EnzeD/vibe-coding) | ⭐4.6k 终极 Vibe Coding 指南，Planning is Everything |
| [[datawhalechina/vibe-vibe]](https://github.com/datawhalechina/vibe-vibe) | ⭐5.4k 中文系统教程，在线阅读 www.vibevibe.cn |
| [[obviousworks/vibe-coding-ai-rules]](https://github.com/obviousworks/vibe-coding-ai-rules) | 10 个 IDE 的统一规则模板 |
| [[tradecatlabs/vibe-coding-cn]](https://github.com/tradecatlabs/vibe-coding-cn) | 中文最系统的五层工程框架教程 |

### Prompt & 模板

| 仓库 | 用途 |
|------|------|
| [[vibe-coding-prompt-template]](https://github.com/KhazP/vibe-coding-prompt-template) | ⭐2.5k 全套 Prompt + 6 个 Claude Code Skills v2.4.0 |
| [[forrestchang/andrej-karpathy-skills]](https://github.com/forrestchang/andrej-karpathy-skills) | Karpathy 65 行 CLAUDE.md 模板 |
| [[kks0488/vibe-claude]](https://github.com/kks0488/vibe-claude) | 极简增强层：5 文件 + 2 Hook + 5 规则 |

### Awesome Lists

| 仓库 | 用途 |
|------|------|
| [[awesome-vibe-coding]](https://github.com/filipecalegario/awesome-vibe-coding) | ⭐4.7k 最全精选合集 |
| [[taskade/awesome-vibe-coding]](https://github.com/taskade/awesome-vibe-coding) | 285+ 工具，按经验等级分级 |
| [[hacrex/the-vibe-coding-handbook]](https://github.com/hacrex/the-vibe-coding-handbook) | Zero to Hero 完整手册 |

### NPM 工具

| 工具 | 用途 |
|------|------|
| `npx agent-rules-sync` | 一次编写规则 → 同步所有 AI 编程工具 |
| `npx codingwithagent` | 交互式生成 Minimal/Standard/Strict 规则配置 |

---

## 🔗 相关笔记

- [[ai agent]]
- [[Claude Code]]
- [[vibe-vibe]]
